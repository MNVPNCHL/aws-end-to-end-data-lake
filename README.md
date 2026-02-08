# AWS End-to-End Data Lake for YouTube Analytics

## Overview
This project implements an end-to-end AWS data engineering pipeline to ingest, transform, and publish analytics-ready YouTube trending datasets using a multi-layer data lake design (Raw → Cleansed → Conformed).

## Architecture
![Architecture]()

## Tech Stack
- Amazon S3 (Raw/Cleansed layers)
- AWS Lambda (S3-triggered JSON processing)
- AWS Glue (PySpark ETL)
- AWS Glue Data Catalog
- Amazon Athena / Amazon Redshift (analytics)

## Data Flow
1. Upload raw CSV files to S3 using Hive-style partitions: `region=us`, `region=ca`, etc.
2. Lambda triggers on JSON uploads, normalizes nested JSON, converts to Parquet, and writes to cleansed S3 + updates Glue Catalog.
3. AWS Glue PySpark job reads raw table from Glue Catalog, applies mapping/cleaning, partitions output by region, and writes Parquet to cleansed S3.

## Repository Structure
- `src/lambda/` → Lambda function (awswrangler + pandas)
- `src/glue/` → AWS Glue PySpark ETL script
- `src/cli/` → AWS CLI upload scripts
- `architecture/` → architecture diagram

## How to Run (High Level)
1. Upload raw data to S3 (`src/cli/s3_upload_commands.sh`)
2. Configure Lambda environment variables (see `sample-config/lambda_env_example.json`)
3. Create Glue Crawlers / Catalog tables
4. Run Glue ETL job and query results using Athena/Redshift

## Key Highlights
- Event-driven ingestion (S3 → Lambda)
- JSON normalization + Parquet conversion
- Glue PySpark transformations + schema enforcement
- Partitioning + predicate pushdown optimization
