Sales Data Pipeline

A Databricks-based sales data pipeline built on AWS S3 and following the Medallion Architecture. The pipeline processes sales data from the child company through Bronze, Silver, and Gold layers, with the Gold child-company data integrated into a parent-company Gold layer for consolidated reporting and analytics.

Architecture
                         AWS S3
                           │
                           ▼
                    ┌─────────────┐
                    │    Bronze   │
                    │ Raw Sales   │
                    │    Data     │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │    Silver   │
                    │ Cleaned &   │
                    │ Transformed │
                    │    Data     │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │ Gold Child  │
                    │ Company     │
                    │ Sales Data  │
                    └──────┬──────┘
                           │
                           │ Integration
                           ▼
                    ┌─────────────┐
                    │ Gold Parent │
                    │   Company   │
                    │ Consolidated│
                    │    Data     │
                    └──────┬──────┘
                           │
                           ▼
                    ┌─────────────┐
                    │  Dashboard  │
                    │  & Analytics│
                    └─────────────┘
Overview

This project demonstrates an end-to-end sales data engineering pipeline using Databricks and Amazon S3.

The child company sales data is sourced from an S3 bucket and processed through the Medallion Architecture:

Bronze Layer — Ingests and stores raw data.
Silver Layer — Cleans, validates, and transforms the raw data.
Gold Layer — Produces business-ready sales data for the child company.
Parent Gold Layer — Integrates Gold-level data from the child company with the parent company's data to support consolidated analytics.

The project supports both full-load and incremental-load processing.

Key Features
Full Load

The full-load process is used to initially ingest the available historical sales data from the S3 bucket.

S3
 │
 ▼
Bronze
 │
 ▼
Silver
 │
 ▼
Gold Child
 │
 ▼
Gold Parent

This provides a complete historical dataset for downstream analytics.

Incremental Load

After the initial full load, the pipeline can process newly arrived or changed data incrementally rather than reprocessing the entire dataset.

New/Updated Data in S3
          │
          ▼
       Bronze
          │
          ▼
       Silver
          │
          ▼
     Gold Child
          │
          ▼
     Gold Parent

Incremental processing helps reduce processing time and unnecessary compute by focusing on new or changed records.

Medallion Architecture
🥉 Bronze Layer

The Bronze layer contains the raw sales data ingested from the S3 bucket.

Responsibilities include:

Ingesting source data
Preserving the original source information
Adding ingestion metadata where required
Supporting both historical and incremental ingestion

The Bronze layer acts as the foundation for downstream processing.

🥈 Silver Layer

The Silver layer contains cleaned and standardized sales data.

Typical transformations include:

Data type standardization
Handling missing or invalid values
Removing duplicates
Data validation
Standardizing business fields
Applying transformation and cleansing rules

The goal is to provide reliable and consistent data for business-level transformations.

🥇 Gold Layer — Child Company

The Child Company Gold layer contains business-ready sales data designed for reporting and analytics.

The Gold layer represents the final analytical view of the child company's sales data.

🥇 Gold Layer — Parent Company

The child company's Gold data is integrated into the Parent Company Gold layer after checking which months were affected.

This enables the parent company to combine the child company's sales information with other company-level data and create a consolidated view.

Child Company
     │
     └── Gold Sales Data
              │
              ▼
       Parent Company
              │
              └── Gold Layer
                    │
                    ▼
             Consolidated Data

This integration allows parent-level reporting and analytics while maintaining the child company's dedicated data pipeline.

Data Source

The source data is stored in an AWS S3 bucket.

AWS S3 Bucket
    │
    ├── Historical Data
    │
    └── Incremental Data

Databricks is responsible for ingesting and processing the data through the different Medallion layers.

Technology Stack
Technology	Purpose
AWS S3	Source data storage
Databricks	Data ingestion, transformation, and processing
Apache Spark	Distributed data processing
Delta Lake	Reliable storage and table management
GitHub	Source-code version control
Databricks SQL / Dashboards	Data visualization and analytics
Pipeline Flow

The overall pipeline can be summarized as:

                ┌──────────────┐
                │    AWS S3    │
                │ Sales Source │
                └──────┬───────┘
                       │
                 Full / Incremental
                       │
                       ▼
                ┌──────────────┐
                │    Bronze    │
                │     Raw      │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │    Silver    │
                │ Clean & Prep │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ Gold - Child │
                │   Company    │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │ Gold - Parent│
                │   Company    │
                └──────┬───────┘
                       │
                       ▼
                ┌──────────────┐
                │  Dashboard   │
                │   Example    │
                └──────────────┘
Dashboard

A dashboard is provided as an example of how the processed Gold-layer data can be consumed for business analytics.

The dashboard demonstrates how the pipeline can support visualization of sales-related metrics and KPIs.

The dashboard is intended to demonstrate the consumption layer of the data pipeline rather than represent a production reporting solution.


A typical business flow is:

Sales data is delivered to the S3 bucket.
Databricks ingests the source data into the Bronze layer.
The Silver layer cleans and standardizes the data.
Business transformations are applied to produce the child company's Gold dataset.
The child company's Gold data is integrated into the parent company's Gold layer.
The consolidated data is made available for analytics.
A dashboard consumes the analytical data to present business KPIs and trends.
Project Purpose

This project demonstrates an end-to-end modern data engineering architecture for processing sales data using cloud storage and Databricks.

It showcases:

Cloud-based data ingestion
Medallion Architecture
Full and incremental data processing
Data transformation and cleansing
Child-to-parent data integration
Analytical Gold-layer design
Dashboard-based data consumption
Git-based project management
