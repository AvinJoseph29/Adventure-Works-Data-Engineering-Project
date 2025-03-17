# Adventure-Works-Data-Engineering-Project


# Azure Data Engineering Project

This project demonstrates a complete **ETL pipeline** using Azure services, including **Azure Data Lake, Azure Data Factory (ADF), Databricks, and Synapse Analytics**. The goal is to extract, transform, and load data from GitHub into a structured data warehouse, leveraging **bronze, silver, and gold layers** for efficient data processing.

![Untitled presentation (2)](https://github.com/user-attachments/assets/21c2e512-fd13-4f99-a2f0-dd04356a581a)



## 📌 Project Overview

### 🔹 Azure Resources Used
1. 🏗 **Resource Group** – Organizes all the services under one group.
2. 💾 **Azure Data Lake Storage** – Stores raw, processed, and transformed data.
3. 🔄 **Azure Data Factory (ADF)** – Handles data ingestion and movement.
4. 🛠 **Azure Databricks** – Processes data with transformations.
5. 📊 **Azure Synapse Analytics** – Stores final structured data and integrates with Power BI.

---

## 🏗 Step 1: Setting Up Azure Data Lake Storage

1. **Create a Resource Group** – This groups all related Azure services together.
2. **Create an Azure Data Lake Storage Account**:
   - Navigate to **Azure Portal → Storage Accounts**.
   - Click **Create Storage Account**, select your resource group, and provide a unique name.
   - Enable **Hierarchical Namespace** for optimized big data operations.
3. **Create Containers in Data Lake**:
   - Go to **Data Lake Storage Account → Containers** and create:
     - 🟤 **Bronze** – Stores raw, unprocessed data.
     - ⚪ **Silver** – Cleaned and transformed data.
     - 🟡 **Gold** – Final structured data for analytics.



---

## 🏗 Step 2: Configuring Azure Data Factory (ADF)



### 🔹 Creating the Data Pipeline
1. Open **ADF Studio** → **Author Tab** → Click **New Pipeline**.


2. Under **Activities → Move & Transform**, drag and drop the **Copy Data** activity.
Source - Data source , sink - destination



3. **Configure Source and Sink**:
   - **Source:** GitHub dataset (CSV file).
   - **Sink:** Azure Data Lake (Bronze Container).



### 🔹 Setting Up Linked Services
1. **Create a Linked Service for GitHub** (to pull the data from github , since github data is http, we use the http connection):
   - Choose **HTTP Connector**.

   - Provide **Base URL**: `https://raw.githubusercontent.com/` (till.com)
   - Use relative path: `anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv`


2. **Create a Linked Service for Data Lake**:
   - Choose **Azure Data Lake Storage Gen2**.
   - Provide authentication details.



Now we need to add the source dataset, click on new besides source dataset




In set properties , provide name, pick the inked service (http one) we created earlier , and also provide the relative url to our data which is in git ie everything after .com ie anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv



Now we go on to sink

Click on Azure data lake







### 🔹 Running the Pipeline
1. Click **Debug** to test pipeline execution.




2. Verify the file in **Bronze container** in Data Lake.


3. Click **Publish** to save the pipeline.



---

## 🏗 Step 3: Building Dynamic Pipelines in ADF
(rather than dragging and dropping the copy data again and again , instead we would use iterations and conditions)

Let’s create a new pipeline 




### 🔹 Configuring Parameters
1. Define **three parameters**:
   - `RelativeLink` – Dynamic GitHub dataset link.(since it keeps changing all the time and it is different for different datasets)
   - `FolderName` – Folder in Data Lake. (eg : products folder)
   - `FileName` – CSV file name. (eg : product.csv)

Click Advanced in Set Properties


Click add dynamic content 



Click add parameters










Now for sink as well , do the same 


In the file path , just add the container name ie bronze since its always same/static
Then add dynamic content for directory , add a new parameter


Similarly for file name add dynamic content , create a new parameter








Drag and drop the for each activity






2. Modify source and sink properties to accept **dynamic content**.

### 🔹 Using ForEach Activity
1. **Create a JSON file** containing dataset parameters.

2. **Upload JSON file** to Data Lake., for this create a new container in your data lake resource


3. **Use Lookup Activity** to read JSON 

Now , inorder to pull this data into adf, we need to add a new activity called lookup (output of the data)


Go to settings  -> source dataset -> add new -> data lake ->json format





Uncheck the first row only box


Now i just want to see the output of the lookup , for thst i will need to deactivate the other 2 activities ie ForEachGit and DynamicCopy


Now run it , only lookup will run now


Click the exit button near the activity name lookupGit to view the output


Click on add dynamic content in the forEachGit Activity


Click on lookupGit


Now in the output we need to access the value variable 


Now we’re getting the array from the lookup activity into the foreach activity . Cut and copy the dynamiccopy activity . Click on the pencil symbol as shown down , which opens a new canvas , now paste the dynamiccopy activity there



Pate the activity here , then in source’s  value add dynamic content





For source, we have 1 parameter that i relative url

Similarly for sink , add dynamic content , and add th parameters ie file name and folder name parameter


 
Then RUN



Now you can verify it , by going to bronze container in azure data lake


Now we’re done with building pipeline in ADF ,


---

## 🏗 Step 4: Data Transformation with Azure Databricks

### 🔹 Connecting Databricks with Data Lake
1. **Create an Azure Databricks workspace**.





Compute is a cluster , click compute -> create a compute

 



Now basically , we’re going to ingest the data from bronze layer to Silver layer using databricks with some transformation



.
2. **Generate a secret key** for authentication.
We’ll see how do we connect the data from datalake to databricks , we need access-level permission .  We neeed to make the key that would connect both of them







Save the above information in a notepad 
Now we need to create a secret key 


Copy the value and secret id and add it to your notepad



3. **Configure access-level permissions** in Data Lake
Once create , now we need to manage access control in our data lake






Now let’s launch our azure databricks workspace and create a new folder, then create a notebook


4. **Use Microsoft Documentation Code Snippet** to establish connection.
Now we connect the azure data lake with the databricks , get the code snippet from microsoft documentation -  (step 7) - https://learn.microsoft.com/en-us/azure/databricks/connect/storage/tutorial-azure-storage




📌 *Screenshot: Configuring Secrets in Databricks*

### 🔹 Transforming Data (Bronze to Silver Layer)
1. **Read Bronze layer data** into a Databricks notebook.

2. **Apply transformations** (data cleaning, filtering, aggregations).
3. **Convert data to Parquet format**.
4. **Write transformed data** to the **Silver container** in Data Lake. (refer code)



---

## 🏗 Step 5: Structuring Data with Azure Synapse Analytics

Now we go into the next phase ie gold layer using azure synapse analytics . create a resource of it 

### 🔹 Setting Up Synapse Analytics
1. **Create an Azure Synapse Analytics workspace**.




While creating the azure synapse resource , we’ve to create a new storage acc( data lake storage) for this 




Once deployed , go to resource group





Azure synapse analytics is a unified platform

When you navigate to Integrate , you can see a similar ui as ADF which has similar features



When you navigate to develop , you can perform any kind of scripts be it sql or spark/notebook (we won’t call it as databricks)



When you navigate to data , you can add database be it sql (create tables , fact/dim tables , data warehouse) or external data
Lake database - in spark when you create the data it is called lakehouse(in databricks) or lake database (in synapse)

Synapse can be connected with data lake without creating any key as we did before , since both of them are azure products. Synapse itself by default has credentials(System manage Identity/Managed Identity)


### 🔹 Connecting Synapse to Data Lake
1. Use **Managed Identity Authentication** to access Data Lake.
2. Configure **IAM role assignments** in Data Lake.
3. Provision a serverless SQL pool
Now go to your data lake in your resource group to IAM , to add role assignment




Now go to your synapse workspace and create a SQL script

Now let’s create the sql db first 


Now we’ll choose serverless , since our data resides in the lakehouse and not physically in any database . cost is less when you store data in lakehouse . 


Now got to your sql script , under use database dropdown you’ve option fot your newly created db ie awdatabase

Provide access to yourself in data lakehouse storage , to access the data , IAM -> ADD ROLE ->  search storage blob data contributor 



### 🔹 Querying Data in Synapse

1. Use `OPENROWSET` to read Parquet data from **Silver Layer**.
OPENROWSET function in our sql script

To get the link of your data file go to your datalake storage -> silver 


We need not copy the entire url , only till folder name is required


Here in the link it says blob.core , but we’re using datalake , so we’ve to change it to dfs (data file storage)


With the help openrowset function you can return the parquet data asa tabular format




2.. Create **views** for frequently accessed datasets.

Similarly create views for all the datafiles





📌 *Screenshot: Querying Data in Synapse SQL Pool*

### 🔹 Creating External Tables
1. **Create a Database Master Key**.
2. **Define Database Scoped Credentials**.
3. **Create External Data Sources** for Silver and Gold layers.
4. **Define External File Format** for Parquet.
5. **Move data from Silver to Gold using `CETAS`**.
6. Verify structured data in **Gold container**.

we’ve to create external tables in synapse, it involves 3 steps : - credential; - external data source; - external file format 

Create a new sql script - Create External Table



Firstly we’ll have to create a master key


Create database scoped credential ( from microsoft documentation refer the code)


Now let’s create the external datasource , we’ll have 2 creat 2 of them since we need to read data from silver , and push data to gold




Now let’s go to 3rd step ie create external file format


Now that we’ve created a view on silver , we’ll push the data from silver to gold to crete an external table using CETAS (Create External Table As) 





When we create External table , it is actually being stored in our datalake







---

## 🏗 Step 6: Visualizing Data with Power BI

### 🔹 Connecting Power BI to Synapse
1. Retrieve **SQL Endpoint** from Synapse.
Now we got to establish conncection between synapse and PowerBI, For that we require sql endpoints



Now copy that endpoint and go to powerbi -> get data

2. Open **Power BI → Get Data**.



3. Paste the **SQL Endpoint** to establish a connection.
Paste the sql endpoint



4. Build **dashboards and reports**.



---

## 🎯 Conclusion
This project successfully implements an **end-to-end ETL pipeline** using Azure services. The structured approach ensures **scalable and efficient data processing**:
- **Raw data ingestion** from GitHub (**Bronze Layer**).
- **Data transformation** using Databricks (**Silver Layer**).
- **Data structuring & querying** in Synapse (**Gold Layer**).
- **Data visualization** in Power BI.

This setup is ideal for real-world **data engineering workflows** with automated ingestion, transformation, and reporting capabilities.


