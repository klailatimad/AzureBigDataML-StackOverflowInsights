# Data Ingestion

## Data Ingestion Diagram

![Data Ingestion Diagram](data_ingestion_diagram.png)

### Steps

1. **Connect to PostgreSQL**: Use ADF to transfer Users and PostTypes tables to the Data Lake.
2. **Connect to Azure Blob Storage**: Use ADF to transfer daily Posts data to the Data Lake.

### Data Factory Creation

In this part, you have two main tasks:

1. **Use Azure Data Factory to ingest data from 2 data sources**: a PostgreSQL database on RDS and WeCloudData's public Azure storage container.
2. **Build your data lake on Azure** and store the ingested data files in the data lake.

#### 1.1. Data Lake

- Create a storage account, a container, and folders to accept the files from the data sources.
- Enable hierarchical namespace to create a data lake instead of simple blob storage, which allows folder creation in the container.

You can name your container as `bd-project` and create folders to manage the data files efficiently:

- **postTypes**: Accepts the PostTypes table from PostgreSQL.
- **Users**: Accepts the Users table from PostgreSQL.
- **Posts**: Accepts today's Parquet files from WeCloudData's public blob storage.

#### 1.2. Data Factory

##### Pipelines

1. **Pipeline 1: `copyOnceWeek`**
   - Set a schedule to copy data once a week.
   - Empty the folder before copying new files.
2. **Pipeline 2: `copyPostsEveryday`**
   - Used to copy daily Posts Parquet files from WeCloudData's public blob storage to your storage.
   - Run it every day and empty the folder before copying new files.

##### Linked Services

1. **Linked Service 1: `ls_rds_pg`** (PostgreSQL RDS)
   - Host: `ENTER YOUR HOST URL`
   - Master Username: `postgres`
   - Password: `weclouddatade`
   - Database: `stack`
   - Port: `5432`
2. **Linked Service 2: `ls_wcd_blob`** (WeCloudData's public storage blob)
   - Storage Account Access Key: `ENTER YOUR STORAGE ACCOUNT ACCESS KEY`
3. **Linked Service 3: `ls_my_blob`** (Your own storage blob)

##### Datasets

1. **Pipeline 1**:
   - Two datasets representing the PostTypes and Users tables on PostgreSQL.
   - Two datasets representing the PostTypes and Users files copied from PostgreSQL to your blob storage.
2. **Pipeline 2**:
   - A dataset representing the Posts Parquet files on WeCloudData's storage blob.
   - A dataset representing the Posts Parquet files on your own storage blob.

##### Activities

1. **Pipeline 1**:
   - Two copy activities to copy both tables from PostgreSQL.
   - Two delete activities to empty the folders before copying new files weekly.
2. **Pipeline 2**:
   - One copy activity to copy Posts Parquet files from WeCloudData's blob storage to your blob storage.
   - One delete activity to empty the folder before copying new files daily.

##### Test Run

- Test run both pipelines by clicking Debug. Ensure files are copied successfully to your blob storage.
- Set schedules for both pipelines: 
  - Pipeline 1: Weekly.
  - Pipeline 2: Daily.

- **Publish** your pipelines to save them in the system.

### Images

- Data Factory Creation Steps
  - ![Step 1](/azure_bd_ml_project/5_data_ingestion/az_bd_ppl1.jpg)
  - ![Step 2](/azure_bd_ml_project/5_data_ingestion/az_bd_ppl2.jpg)
  - ![Step 3](/azure_bd_ml_project/5_data_ingestion/bd_diagram_ingest.png)
