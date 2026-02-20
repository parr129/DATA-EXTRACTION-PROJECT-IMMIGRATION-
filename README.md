
# Data Engineering  Project

## Table of Contents
* Project Overview:
    This project showcases key competencies in data engineering, including building scalable data ingestion architecture with Airflow and Spark, establishing cloud-based data warehouses using Redshift and S3 storage, and implementing efficient star schema database models. The project examines I94 US immigration patterns, airport operations, port of entry cities, and regional weather data, with emphasis on visa categories and associated demographic profiles. The scope encompasses the data sources listed below, aggregated across multiple dimensions including visa types, gender, port of entry, nationality, and time periods.
    * Which airlines transport immigrants to the United States?
    * Which cities receive these arrivals?
    * What are the prevailing weather conditions in these destinations?
    * For comprehensive details and additional analysis, refer to the data exploration section.

## Data Sources & Description:
* I94 Immigration Data: This dataset originates from the US National Tourism and Trade Office, accessible [here](https://travel.trade.gov/research/reports/i94/historical/2016.html). Each report provides international visitor arrival statistics by geographic region and individual countries (including the leading 20), visa categories, transportation methods, age demographics, states of initial residence, and primary ports of entry for selected countries.
* U.S. City Demographic Data: This dataset provides demographic information for all US cities and census-designated places with populations of 65,000 or more. The source is OpenSoft, accessible [here](https://public.opendatasoft.com/explore/dataset/us-cities-demographics/export/).
* Airport Code Table: This table lists airport codes paired with their corresponding cities. Airport codes may be IATA codes (three-letter codes used in passenger reservation and baggage systems) or ICAO codes (four-letter codes used by air traffic control and for airports lacking IATA codes). Source available [here](https://datahub.io/core/airport-codes#data).
* World Temperature Data: This temperature dataset is sourced from Kaggle and available [here](https://www.kaggle.com/berkeleyearth/climate-change-earth-surface-temperature-data).

## Design Goals:
During the initial project planning phase, I established architectural principles to guide development and ensure a comprehensive, accurate, dependable, and expandable data lineage system for immigration data. Key principles include:
1. Maintain data integrity — Accurately map relationships between data from multiple sources to establish credibility, as unreliable lineage information can be counterproductive.
2. Facilitate smooth integration — Build the system to work with an expanding ecosystem of data tools and platforms, including those without native metadata capabilities.
3. Create an adaptable data structure — Use a generic model to represent diverse data types and their interconnections, supporting various analytical scenarios.

## Data Exploration:
Additional analysis details are available in:
* *Final_Notebook*: Contains consolidated findings and analysis [here](Final_Notebook.ipynb).  
* *Immigration_data_exploration*: Focuses on immigration data analysis [here](Immgration_data_exploration.ipynb).
* *PortOfEntry_data_exploration*: Covers port of entry analysis [here](PortOfEntry_data_exploration.ipynb).
* *World_Temperature_data_exploration*: Examines temperature pattern data [here](World_Temperature_data_exploration.ipynb).
* *Airport_data_exploration*: Analyzes airport operations data [here](Airport_data_exploration.ipynb).
    
* Note: Data originates from diverse sources including log systems, data warehouses, and third-party APIs. It is essential to examine data structure, size, granularity, frequency, quality, and interdependencies.    
    
## Data Modeling:
Following comprehensive data exploration from multiple sources—including I94 Immigration Data (immigration codes file `I94_SAS_Labels_Descriptions.SAS`), U.S. City Demographic Data, Airport Codes, and World Temperature Data—I developed a star schema with the immigration fact table and corresponding dimension tables as illustrated below:
<img src="./images/schema.png"/>


## ETL Pipeline Framework:
  * ETL Design Principles: The following design principles were implemented during pipeline development:
    1. Variability in data size and frequency
    <p align="center"><img src="./images/Variation_in_data_size_and_cadency.png" style="height: 100%; width: 100%; max-width: 90%"/></p>

    2. Variability in raw data accessibility
    <img src="./images/Variation_in_access_to_raw_data.png"/>

    3. Variability in file format types
    <img src="./images/Variation_in_file_formats.png"/>

    4. Coverage of multiple I94 system aspects
    <img src="./images/Cover_several_aspects_of_I94_system.png"/>

    5. Security and data protection
    <img src="./images/Security_and_Privacy.png"/>

    6. Airflow-scheduled workflows
    <img src="./images/Airflow_Schedule_workflows.png"/>

    7. Spark-based distributed data processing
    <img src="./images/Spark_Distributed_Processing.png"/>

    8. Validation of data quality
    <img src="./images/Data_Quality.png"/>

The data model and star schema implementation rely heavily on Airflow's orchestration capabilities. The process involves extracting files from S3 buckets, transforming data with Spark, and storing results in PARQUET format. The resulting PARQUET files are then written to Redshift through a series of coordinated ETL tasks, as illustrated in the DAG graph below. Key steps include:
- Reading SAS documents and converting them to CSV files in the S3 immigration bucket
- Loading additional CSV and PARQUET files from the S3 immigration bucket
- Converting PARQUET files to star schema dimension tables stored in the S3 transformed bucket
- Writing transformed PARQUET files to Redshift
- Running validation checks on newly created tables
- The complete ETL pipeline is shown below
  <img src="./images/FinalDataPipeline.png"/>  
 
## Data Storage
Data was stored in S3 buckets as PARQUET files. Given that the immigration dataset comprises several million rows, PARQUET format was selected for its efficiency in data manipulation and processing through Dask, and to facilitate seamless loading into Redshift with appropriate distribution keys.

## Conclusion
This project demonstrates the essential components of building a highly scalable data warehouse. Key skills developed include:
* Understanding the problem domain and data characteristics
* Performing thorough data exploration to assess structure, volume, granularity, quality, relationships, and update frequency
* Provisioning Redshift clusters, configuring IAM roles, and establishing security groups
* Building ETL pipelines that transfer data from S3 into staging tables for star schema processing
* Designing star schemas optimized for the analytics team's specific query requirements
* Using Airflow to orchestrate and automate ETL workflows with Python, Spark, and Amazon Redshift
* Developing custom operators for data staging, warehouse population, and quality validation
* Converting multi-source data into analytics-ready star schema structures
