# Data Transformation

## Data Transformation Diagram

![Data Transformation Diagram](/azure_bd_ml_project/6_data_transformation/bd_diagram_transform.png)

### Steps

1. **Data Cleaning and Transformation**: Use Databricks to process raw data.
2. **Machine Learning**: Train and apply models to classify post topics.

### Azure Databricks Integration

#### Steps to Integrate Databricks with Azure Storage

1. **Create an Azure Databricks Workspace, Compute Cluster, and Notebook**:
   - Search for Databricks in the Azure home page and create a new workspace.
   - Create a compute cluster with the specified settings.
   - Create a new notebook and attach it to the cluster.

2. **Allow Azure Databricks to Access Your Storage Container**:
   - Create an app registration in Azure Entra ID.
   - Note the Application ID and Directory ID.
   - Create a client secret and note its value.
   - Assign the Storage Blob Data Contributor role to the app.

3. **Mount Your Storage Container to the Azure Databricks Directory**:
   - Use the provided script to mount the storage container.

```python
storageAccountName = 'ADD YOUR STORAGE ACCOUNT NAME'
containerName = 'project'
applicationId = 'ADD YOUR APPLICATION ID'
directoryID = 'ADD YOUR DIRECTORY'
secretValue = 'ADD YOUR SECRET'
endpoint = 'https://login.microsoftonline.com/' + directoryID + '/oauth2/token'
source = 'abfss://' + containerName + '@' + storageAccountName + '.dfs.core.windows.net/'
mountPoint = "/mnt/deBDProject"

configs = {"fs.azure.account.auth.type": "OAuth",
           "fs.azure.account.oauth.provider.type": "org.apache.hadoop.fs.azurebfs.oauth2.ClientCredsTokenProvider",
           "fs.azure.account.oauth2.client.id": applicationId,
           "fs.azure.account.oauth2.client.secret": secretValue,
           "fs.azure.account.oauth2.client.endpoint": endpoint}

dbutils.fs.mount(source = source, mount_point = mountPoint, extra_configs = configs)
```

4. **Validate the Mounting**:
```py
display(dbutils.fs.ls("/mnt/deBDProject"))
```
5. **Images**:
-   Databricks Integration Steps:
	- ![Step 1](/azure_bd_ml_project/6_data_transformation/ws_mnt1.jpg)
	- ![Step 2](/azure_bd_ml_project/6_data_transformation/ws_mnt2.jpg)
	- ![Step 3](/azure_bd_ml_project/6_data_transformation/ws_mnt4.jpg)
	- ![Step 4](/azure_bd_ml_project/6_data_transformation/ws_mnt5.jpg)
	- ![Step 5](/azure_bd_ml_project/6_data_transformation/ws_mnt6.jpg)
	- ![Step 6](/azure_bd_ml_project/6_data_transformation/ws_mnt7.jpg)
	- ![Step 7](/azure_bd_ml_project/6_data_transformation/ws_mnt8.jpg)
	- ![Step 8](/azure_bd_ml_project/6_data_transformation/ws_mnt9.jpg)
	- ![Step 9](/azure_bd_ml_project/6_data_transformation/ws_mnt10.jpg)
	- ![Step 10](/azure_bd_ml_project/6_data_transformation/ws_mnt11.jpg)
	- ![Step 11](/azure_bd_ml_project/6_data_transformation/ws_mnt12.jpg)
	- ![Step 12](/azure_bd_ml_project/6_data_transformation/ws_mnt13.jpg)