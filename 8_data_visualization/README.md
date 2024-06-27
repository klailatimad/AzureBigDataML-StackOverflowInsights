# Data Visualization

## Data Visualization Diagram

![Data Visualization Diagram](/azure_bd_ml_project/8_data_visualization/bd_diagram_bi.png)

### Steps

1. **Store Machine Learning Results**

   Ensure the machine learning results file (`ml_output.csv`) is stored in your Azure storage blob container.

   ![ML Output File](/azure_bd_ml_project/8_data_visualization/ml_result.jpg)

2. **Create Synapse Workspace and Link Storage Account**

   Ensure your Synapse workspace includes the storage account containing the machine learning result file.

   ![Synapse Linked Storage](/azure_bd_ml_project/8_data_visualization/syn_sto_acct.jpg)

3. **Run Ad Hoc Query on CSV File**

   - Navigate to the file in Synapse: `Data --> Linked --> <your_storage_account> --> <your_container> --> ml_output.csv`.
   - Right-click the file and select "New SQL script --> Select Top 100 rows".

   ![SQL Script Editor](/azure_bd_ml_project/8_data_visualization/open_a_new_Script.jpg)

4. **Modify the SQL Query**

   Ensure your query includes the header and points to the correct file location:

   ```sql
   SELECT
       TOP 5 *
   FROM
       OPENROWSET(
           BULK 'https://destorage4st.dfs.core.windows.net/project/BI/ml_output.csv',
           FORMAT = 'CSV',
           PARSER_VERSION = '2.0',
           HEADER_ROW = TRUE
       ) AS [result]
	```
5.  **Create Column Chart**  
    -   In the query result, choose to display it as a chart.
    -   Set the chart type to "Column", the category column to "topic", and the legend column to "qty".
    
6.  **Result**
    Congratulations! You have successfully visualized the top 5 topics from StackOverflow posts.