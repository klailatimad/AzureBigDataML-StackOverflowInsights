# Business Requirements

## Data Lake Requirements

- Create a Data Lake using Azure Storage Blob with hierarchical namespace.
- Use Azure Data Factory (ADF) to manage data ingestion and processing.

## Data Ingestion

- **RDS PostgreSQL**: Ingest Users and PostTypes tables weekly using SCD Type 1 (overwrite old records).
- **Azure Blob Storage**: Ingest daily Posts data in Parquet format.

## Machine Learning Process Requirements

- Develop a Databricks notebook for data processing and machine learning.
- Train a model to classify post topics based on text content.
- Output daily topics and their counts, sorted by frequency.

### Steps Involved

1. **Data Loading**: Load the data from the Azure Data Lake into Databricks.
2. **Data Cleaning**: Preprocess the data to remove any irrelevant information.
3. **Feature Engineering**: Transform the text data into numerical features that can be used for machine learning.
4. **Model Training**: Train a machine learning model to classify the posts into different topics.
5. **Model Evaluation**: Evaluate the model to ensure it meets the required performance metrics.
6. **Output Generation**: Generate daily topic counts and store the results back into the Azure Data Lake.

This process ensures that the data is clean, transformed, and analyzed to provide meaningful insights into the trending topics on StackOverflow.
