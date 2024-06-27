# Big Data Engineering Project on Azure

Welcome to the Big Data Engineering Project on Azure repository! This project demonstrates a comprehensive data engineering pipeline using various Azure services, focusing on data ingestion, transformation, machine learning, and visualization. The data is sourced from StackOverflow and processed to provide insightful visualizations of trending topics.

![Project Diagram](https://github.com/klailatimad/AzureBigDataML-StackOverflowInsights/assets/122483291/f3f7b042-4f31-4222-9338-fa6c5f63aa3a)

## Table of Contents

1. [Project Overview](./1_project_overview/README.md)
2. [About Data](./2_about_data/README.md)
3. [Business Requirements](./3_business_requirements/README.md)
4. [Project Infrastructure](./4_project_infrastructure/README.md)
5. [Data Ingestion](./5_data_ingestion/README.md)
6. [Data Transformation](./6_data_transformation/README.md)
7. [Machine Learning](./7_machine_learning/README.md)
8. [Data Visualization](./8_data_visualization/README.md)
9. [Example Data](#example-data)
10. [Training Data](#training-data)

## Quick Start

To get started with this project, follow the steps below:

### 1. Clone the Repository

```sh
git clone https://github.com/klailatimad/AzureBigDataML-StackOverflowInsights.git
cd AzureBigDataML-StackOverflowInsights
```
### 2. Set Up Azure Services

Ensure you have an Azure account and the necessary permissions to create and manage resources. Follow the instructions in the Project Infrastructure section to set up your Azure environment.

### 3. Data Ingestion

Refer to the Data Ingestion section for detailed steps on setting up Azure Data Factory and ingesting data from the specified sources.

### 4. Data Transformation and Machine Learning

Follow the instructions in the Data Transformation and Machine Learning sections to process and analyze the data using Azure Databricks.

### 5. Data Visualization

Finally, use Azure Synapse to visualize the processed data as described in the Data Visualization section.


## Example Data

To help you get started, we have included some example parquet files that represent the data used for daily ingestion. You can find these files in the `example_data` folder.

### Using the Example Data

1.  **Upload Example Data to Azure Blob Storage**: Upload the files from the `example_data` folder to your Azure Blob Storage container.
    
2.  **Configure Azure Data Factory**: Configure your Azure Data Factory pipeline to use the example data files for ingestion.
    
3.  **Run the Pipeline**: Run the pipeline to ingest the example data into your data lake.

By following these steps, you can simulate the data ingestion process using the provided example data files.


## Training Data

We have also included a larger dataset for training the machine learning model. You can find this data in the `training_data` folder as a zip file.

### Using the Training Data

1.  **Unzip the Training Data**: Unzip the file in the `training_data` folder.
    
    ```sh   
    unzip training_data/training_data.zip -d training_data/ 
    ```
2.  **Upload Training Data to Azure Blob Storage**: Upload the unzipped files to your Azure Blob Storage container.
    
3.  **Configure Databricks**: Configure your Databricks environment to use the training data for model training.
    
4.  **Train the ML Model**: Follow the instructions in the Machine Learning section to train your model using the provided data.