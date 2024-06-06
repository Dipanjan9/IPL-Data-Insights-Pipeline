## IPL-Data-Insights-Pipeline
# Project Overview
Welcome to the IPL Data Insights Pipeline project! This project demonstrates the creation of a robust data pipeline that ingests, processes, and visualizes IPL (Indian Premier League) data. The pipeline leverages various technologies including Amazon S3, Databricks, Apache Spark, Hive, and Power BI to provide insightful analytics on IPL matches, players, and teams.

# Table of Contents
Introduction
Data Source
Architecture
Technologies Used
Setup Instructions
Data Processing
Data Analysis
Data Visualization
Conclusion
Future Enhancements
# Introduction
The IPL Data Insights Pipeline project involves creating an end-to-end data pipeline that:

Ingests IPL data into Amazon S3.
Processes the data using Databricks and Apache Spark.
Stores the processed data in Hive Metastore.
Executes complex queries to derive meaningful insights.
Visualizes the data in Power BI.
Data Source
The IPL data includes detailed information about matches, players, ball-by-ball events, and teams. This dataset serves as the foundation for analysis and visualization in the project.

# Architecture

The pipeline consists of the following steps:

Data Ingestion: Load raw IPL data into Amazon S3.
Data Processing: Use Databricks and Apache Spark for data cleaning, transformation, and enrichment.
Data Storage: Save processed data into Hive Metastore as relational tables.
Data Querying: Perform complex SQL queries to extract useful insights.
Data Visualization: Connect Databricks to Power BI using DirectQuery and create visualizations.
Technologies Used
Amazon S3: For data storage and ingestion.
Databricks: For data processing and transformation.
Apache Spark: For distributed data processing.
Hive Metastore: For storing processed data in relational tables.
Power BI: For data visualization and analytics.
Setup Instructions
Follow these steps to set up the project on your local environment:

# Clone the Repository:

bash
Copy code
git clone https://github.com/yourusername/ipl-data-insights-pipeline.git
cd ipl-data-insights-pipeline
Setup Databricks:

Create a Databricks account if you don't have one.
Create a new cluster and configure it as needed.
Import the provided notebooks into your Databricks workspace.
Load Data into S3:

Upload the IPL data files to an S3 bucket.
Update the data paths in the Databricks notebooks to point to your S3 bucket.
Run Data Processing Notebooks:

Open and run the notebooks in Databricks to process the data and save it to Hive Metastore.
Connect Power BI to Databricks:

Open Power BI Desktop.
Connect to your Databricks cluster using DirectQuery.
Load the Hive tables into Power BI.
Data Processing
The data processing involves several steps:

Data Cleaning: Remove duplicates, handle missing values, and standardize formats.
Data Transformation: Join tables, aggregate data, and create new calculated fields.
Data Enrichment: Add derived metrics and features for analysis.
Example processing script:

python
Copy code
# Example Spark SQL query for data processing
processed_df = spark.sql("""
    SELECT 
        p.player_name,
        m.season_year,
        SUM(b.runs_scored) AS total_runs 
    FROM ball_by_ball b
    JOIN match m ON b.match_id = m.match_id   
    JOIN player_match pm ON m.match_id = pm.match_id AND b.striker = pm.player_id     
    JOIN player p ON p.player_id = pm.player_id
    GROUP BY p.player_name, m.season_year
    ORDER BY m.season_year, total_runs DESC
""")
processed_df.write.mode("overwrite").saveAsTable("IPL_DATA_Warehouse.top_scoring_batsmen_per_season")
# Data Analysis
Perform complex SQL queries to derive insights:

Top Scoring Batsmen Per Season:
sql
Copy code
SELECT 
  p.player_name,
  m.season_year,
  SUM(b.runs_scored) AS total_runs 
FROM IPL_DATA_Warehouse.ball_by_ball b
JOIN IPL_DATA_Warehouse.match m ON b.match_id = m.match_id   
JOIN IPL_DATA_Warehouse.player_match pm ON m.match_id = pm.match_id AND b.striker = pm.player_id     
JOIN IPL_DATA_Warehouse.player p ON p.player_id = pm.player_id
GROUP BY p.player_name, m.season_year
ORDER BY m.season_year, total_runs DESC
Economical Bowlers in Powerplay:
sql
Copy code
SELECT 
  p.player_name, 
  AVG(b.runs_scored) AS avg_runs_per_ball, 
  COUNT(b.bowler_wicket) AS total_wickets
FROM IPL_DATA_Warehouse.ball_by_ball b
JOIN IPL_DATA_Warehouse.player_match pm ON b.match_id = pm.match_id AND b.bowler = pm.player_id
JOIN IPL_DATA_Warehouse.player p ON pm.player_id = p.player_id
WHERE b.over_id <= 6
GROUP BY p.player_name
HAVING COUNT(*) >= 1
ORDER BY avg_runs_per_ball, total_wickets DESC
# Data Visualization
Visualize the processed data in Power BI:

Top Scoring Batsmen Per Season:

Create a bar chart with player_name on the X-axis and total_runs on the Y-axis.
Use season_year to group or color code the bars.
Economical Bowlers in Powerplay:

Create a scatter plot with avg_runs_per_ball on the X-axis and total_wickets on the Y-axis.
Use player_name for data labels.
Conclusion
This project showcases my ability to build a data pipeline from scratch, leveraging various data engineering tools and platforms. By processing and visualizing IPL data, I gained valuable insights and demonstrated my proficiency in handling big data.

# Future Enhancements
Real-time Data Processing: Incorporate streaming data for real-time analytics.
Advanced Analytics: Use machine learning models to predict player performance and match outcomes.
Enhanced Visualizations: Create more interactive and detailed dashboards in Power BI.
Feel free to explore the repository and reach out if you have any questions or suggestions!
