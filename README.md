# Airline Data Ingestion Pipeline - AWS

## Project Overview
A cloud-based ETL pipeline that processes airline flight data, joins it with airport reference data, and loads the results into Amazon Redshift for analytics. The system uses event-driven architecture to automatically process new data files as they arrive.


## Key Features

### Data Sources
- **Airport Dimension Data**: Static reference table with airport codes, names, cities, and states
- **Flight Fact Data**: Daily flight records with carrier, airport codes, and delay information

### AWS Components
- **S3**: Stores raw data using hive-style partitioning (date=YYYY-MM-DD/flights.csv)
- **EventBridge**: Triggers workflow when new CSV files are detected
- **Step Functions**: Orchestrates the data processing workflow
- **Glue Crawlers**: Catalogs metadata for S3 and Redshift tables
- **Glue ETL Job**: Performs transformations including:
  - Filtering flights with significant delays (>60 minutes)
  - Joining flight data with airport dimension data (twice - for origin and destination)
  - Creating a denormalized view for analytics
- **Redshift**: Stores processed data for analytics
- **SNS**: Sends success/failure notifications

### Data Transformation
The ETL process creates a denormalized table with:
- Flight carrier information
- Origin airport details (name, city, state)
- Destination airport details (name, city, state)
- Delay metrics

## Technical Setup Requirements
- VPC endpoints for S3, CloudWatch and Glue
- JDBC connections between Glue and Redshift
- Proper IAM roles and permissions
- Security group configurations

## Implementation
The repository contains:
- CloudFormation/Terraform templates for infrastructure
- SQL scripts for Redshift table creation
- Python scripts for Glue ETL jobs
- Step Function state machine definition
- EventBridge rule patterns

## How to Use
1. Deploy the infrastructure using provided templates
2. Upload airport dimension data to the S3 bucket
3. Load the dimension data into Redshift
4. Upload flight data files to trigger the pipeline
5. Query the transformed data in Redshift

This project showcases a modern, serverless approach to building data pipelines in AWS with proper orchestration, error handling, and incremental processing capabilities.
