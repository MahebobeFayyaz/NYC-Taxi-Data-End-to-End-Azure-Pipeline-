# NYC Taxi Data End-to-End Azure Pipeline 
An end-to-end pipeline for processing NYC Taxi trip data using Microsoft Azure services.

# Project Overview
This project demonstrates an end-to-end data engineering pipeline built on Microsoft Azure for ingesting, transforming, and analyzing NYC Taxi trip data. The pipeline automates the process of downloading taxi trip data, storing it in a Data Lake, transforming it using Databricks, and loading curated datasets into a Gold layer for analytics and visualization.

The architecture follows the **Medallion Architecture (Bronze, Silver, Gold)** to ensure scalable and structured data processing. Azure Data Factory is used for data ingestion, Azure Data Lake Storage Gen2 is used for storage, Databricks performs transformations using PySpark and SQL, and Power BI is used for analytics and visualization.

# Data Source
NYC Taxi & Limousine Commission Data  
https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page

Datasets used in this project:

* Green Taxi Trip Data – Parquet format (downloaded dynamically)
* Taxi Zone Lookup Data – CSV format (downloaded manually)
* Trip Type Data – CSV format (downloaded manually)

# Architecture
The architecture leverages the following Azure services:


* Azure Data Lake Storage Gen2: Stores raw, processed, and curated datasets across Bronze, Silver, and Gold layers.
* Azure Data Factory: Orchestrates ingestion of taxi trip data from the HTTP source into the Data Lake.
* Azure Databricks: Performs large-scale data transformation using PySpark and SQL.
* Microsoft Entra ID: Provides secure authentication for Databricks to access the Data Lake.
* Power BI: Used for visualization and analytics on curated data from the Gold layer.

# Storage Account Setup
A Data Lake Storage account is created with three layers following the Medallion Architecture.

### Bronze Layer (Raw Data)
Stores raw data directly from the source.

Directories:
* trip_type → trip_type.csv
* taxi_zone_lookup → taxi_zone_lookup.csv
* green_taxi_trip_data → monthly parquet files

### Silver Layer
Stores cleaned and transformed datasets in **Parquet format**.

### Gold Layer
Stores curated datasets and Delta tables optimized for analytics and reporting.

# Pipeline Workflow

<img src="YOUR_ARCHITECTURE_IMAGE" />

## Data Ingestion using Azure Data Factory
Azure Data Factory is used to dynamically download Green Taxi Trip data from the HTTP source and load it into the Bronze layer of the Data Lake.

### Configure Linked Services
Two linked services are created:

* Source: HTTP (NYC Taxi Data Website)
* Destination: Azure Data Lake Storage Gen2

<img src="" />

### Data Factory Pipeline
A pipeline is created to download the Green Taxi Trip dataset and load it into the Bronze layer.

Pipeline Flow:

* Source (HTTP)
* Copy Activity
* Sink (Azure Data Lake Storage Gen2 – Bronze Layer)

Pipeline Name: **myWebDL**

<img src="YOUR_PIPELINE_IMAGE" />

The file **green_tripdata_01.parquet** is successfully imported into the Data Lake.

<img src="YOUR_IMPORTED_FILE_IMAGE" />

### Dynamic File Download
To download taxi trip data for all months:

* ForEach Activity is used to iterate through months
* IF Condition checks file availability
* Copy Activity loads available files into Bronze layer

<img src="YOUR_FOREACH_ACTIVITY_IMAGE" />

# Databricks Data Processing

## Secure Access Configuration
To connect Databricks with the Data Lake securely, an application is created in **Microsoft Entra ID**.

Steps performed:

* Created an application in Microsoft Entra ID
* Generated a client secret key
* Assigned **Storage Blob Data Contributor** role to the application

Required credentials:

* Application (Client) ID
* Directory (Tenant) ID
* Client Secret Key

<img src="YOUR_ENTRA_ID_IMAGE" />

## Create Databricks Workspace
A Databricks workspace and compute cluster are created to run transformation jobs.

Workspace Structure:

<img src="YOUR_DATABRICKS_CLUSTER_IMAGE" />

# Bronze to Silver Transformation
Raw data from the Bronze layer is processed in Databricks using **PySpark and SQL**.

Transformations performed:

* Schema validation
* Data cleaning
* Column selection and formatting
* Handling missing values

The transformed data is stored in the **Silver Layer in Delta format**.

<img src="YOUR_SILVER_LAYER_IMAGE" />

# Silver to Gold Transformation
The curated dataset is created from the Silver layer and stored in the Gold layer as **Delta Tables**.

The Gold layer contains analytics-ready data that can be directly used for reporting and business intelligence.

<img src="YOUR_GOLD_LAYER_IMAGE" />

# Power BI Integration
The final Gold layer dataset is connected to Power BI for visualization and analytics.

This allows users to analyze:

* Taxi trip trends
* Trip volume by zones
* Pickup and drop-off insights
* Monthly trip statistics

<img src="YOUR_POWERBI_IMAGE" />

# Tech Stack

* Cloud Platform: Microsoft Azure
* Azure Services: Azure Data Factory, Azure Data Lake Storage Gen2, Azure Databricks, Microsoft Entra ID
* Programming Languages: PySpark, SQL
* Data Formats: Parquet, CSV, Delta Lake
* Visualization: Power BI

# Features

* Dynamic Data Ingestion: Automatically downloads taxi datasets using Azure Data Factory.
* Medallion Architecture: Implements Bronze, Silver, and Gold layers for structured data processing.
* Secure Authentication: Uses Microsoft Entra ID for secure access between Databricks and Data Lake.
* Scalable Data Processing: Uses Databricks for distributed data transformation.
* Delta Lake Integration: Stores transformed data using Delta format for reliability and performance.
* Business Intelligence Integration: Connects curated datasets to Power BI dashboards.

# Getting Started

Prerequisites:

* Microsoft Azure Account
* Azure Data Factory
* Azure Data Lake Storage Gen2
* Azure Databricks Workspace
* Power BI Desktop

Steps to run the project:

1. Create an Azure Data Lake Storage account and configure Bronze, Silver, and Gold layers.
2. Configure Azure Data Factory pipelines to ingest NYC Taxi datasets.
3. Set up Microsoft Entra ID authentication for secure Data Lake access.
4. Create Databricks notebooks for transformation.
5. Process data from Bronze to Silver to Gold layers.
6. Connect the Gold layer dataset to Power BI for analytics and visualization.
