# Project Infrastructure

This project leverages several Azure services to build a scalable and efficient data engineering pipeline. Below is an overview of the key Azure services used and their roles in the project.

## Azure Services Used

### 1. Azure Data Lake

- **Purpose**: Serves as the central storage repository for all ingested data, intermediate data generated during processing, and final output data.
- **Features**: 
  - Hierarchical namespace for organizing data into directories.
  - Scalable storage to handle large volumes of data.

### 2. Azure Data Factory (ADF)

- **Purpose**: Orchestrates the entire data pipeline, managing data ingestion from multiple sources, transforming data, and running machine learning workflows.
- **Features**:
  - Data ingestion from RDS PostgreSQL and Azure Blob Storage.
  - Scheduling and automation of data workflows.
  - Integration with Databricks for data processing and machine learning.

### 3. Azure Databricks

- **Purpose**: Provides a collaborative environment for data scientists and engineers to process data and develop machine learning models.
- **Features**:
  - Scalable computing resources for big data processing.
  - Integration with Apache Spark for distributed data processing.
  - Notebook interface for interactive data analysis and machine learning.

### 4. Azure Synapse

- **Purpose**: Facilitates data analysis and visualization, enabling the creation of reports and dashboards based on the processed data.
- **Features**:
  - SQL-based data exploration and querying.
  - Integration with Azure Data Lake for seamless data access.
  - Visualization tools to create charts and dashboards for business insights.

## Infrastructure Diagram

![Project Diagram](https://github.com/klailatimad/AzureBigDataML-StackOverflowInsights/assets/122483291/b0af095d-1ffc-4494-af36-243b9ceadba7)


## Setting Up the Infrastructure

1. **Create an Azure Data Lake Storage Account**:
   - Enable hierarchical namespace.
   - Create containers and folders to organize data.

2. **Set Up Azure Data Factory**:
   - Create linked services to connect to data sources.
   - Develop and publish pipelines for data ingestion and processing.

3. **Configure Azure Databricks**:
   - Set up a Databricks workspace and create a compute cluster.
   - Mount Azure Data Lake Storage to Databricks for seamless data access.

4. **Utilize Azure Synapse**:
   - Connect to Azure Data Lake Storage.
   - Create and run SQL queries for data analysis.
   - Develop visualizations to present insights.

By leveraging these Azure services, we can efficiently manage the data engineering lifecycle from ingestion and processing to analysis and visualization.
