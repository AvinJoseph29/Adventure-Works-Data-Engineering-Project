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


![image](https://github.com/user-attachments/assets/92231c0d-7b9e-437a-adde-f6913c970eaa)



---


## 🏗 Step 2: Configuring Azure Data Factory (ADF)

![image](https://github.com/user-attachments/assets/30cbbfbb-1338-4456-b6d0-998268fbd3d3)

![image](https://github.com/user-attachments/assets/7fea1ba4-964a-4a9d-9fb8-5770880ff8d4)





### 🔹 Creating the Data Pipeline

1. Open **ADF Studio** → **Author Tab** → Click **New Pipeline**.

![image](https://github.com/user-attachments/assets/3348a2b2-749b-4ce0-a156-b8d069100537)





3. Under **Activities → Move & Transform**, drag and drop the **Copy Data** activity.

Source - Data source , sink - destination

![image](https://github.com/user-attachments/assets/39514f4c-5048-49dc-806c-816ffb2e15e9)



3. **Configure Source and Sink**:

   - **Source:** GitHub dataset (CSV file).

   - **Sink:** Azure Data Lake (Bronze Container).




### 🔹 Setting Up Linked Services

1. **Create a Linked Service for GitHub** (to pull the data from github , since github data is http, we use the http connection):

   - Choose **HTTP Connector**.

![image](https://github.com/user-attachments/assets/f22e7930-65aa-42a7-9625-8a60861e811c)


   - Provide **Base URL**: `https://raw.githubusercontent.com/` (till.com)

   - Use relative path: `anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv`
![image](https://github.com/user-attachments/assets/63c22262-0c43-427a-952b-a06ea1df8694)




2. **Create a Linked Service for Data Lake**:

   - Choose **Azure Data Lake Storage Gen2**.

   - Provide authentication details.
![image](https://github.com/user-attachments/assets/6245d957-0b56-40a7-a4b0-1acc67017b5b)

![image](https://github.com/user-attachments/assets/9aa74e14-d481-4e3c-82c2-86edda329b88)


Now we need to add the source dataset, click on new besides source dataset

![image](https://github.com/user-attachments/assets/191cb29d-9cac-423d-b4b2-18bfb3e3e827)

![image](https://github.com/user-attachments/assets/2f00bacb-f0b2-44ef-9186-d2b1f8648be1)

![image](https://github.com/user-attachments/assets/bf5b8690-2512-4414-b299-919d056e8397)


In set properties , provide name, pick the inked service (http one) we created earlier , and also provide the relative url to our data which is in git ie everything after .com ie [anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv](https://www.google.com/url?q=https://raw.githubusercontent.com/anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv&sa=D&source=editors&ust=1742244078469049&usg=AOvVaw0GWokJ5JOtkr6FITbUc9tG)

![image](https://github.com/user-attachments/assets/d45587e8-141e-447d-b5d3-cd484637eaba)




Now we go on to sink

![image](https://github.com/user-attachments/assets/01d35ca5-971c-4787-bec6-7c1770d8ea94)


Click on Azure data lake

![image](https://github.com/user-attachments/assets/30a8ba03-396b-41de-a15f-8a16c0e437d6)

![image](https://github.com/user-attachments/assets/d36f2894-0c28-49c2-90c1-8c712769cacb)

![image](https://github.com/user-attachments/assets/17b19d52-a7fd-46b7-9eb8-3d7389f7db58)









### 🔹 Running the Pipeline

1. Click **Debug** to test pipeline execution.

![image](https://github.com/user-attachments/assets/b5e9510a-0515-44db-942d-4f5100f7f7a5)

![image](https://github.com/user-attachments/assets/831649cf-9129-42dd-8f0b-78fa41c61c8f)




2. Verify the file in **Bronze container** in Data Lake.

![image](https://github.com/user-attachments/assets/a676b786-ffb5-4cc4-aaae-5b916575d481)



3. Click **Publish** to save the pipeline.




---


## 🏗 Step 3: Building Dynamic Pipelines in ADF

(rather than dragging and dropping the copy data again and again , instead we would use iterations and conditions)


Let’s create a new pipeline

![image](https://github.com/user-attachments/assets/885714b3-35ec-4b40-803a-abf9f7b5c021)


### 🔹 Configuring Parameters

1. Define **three parameters**:

   - `RelativeLink` – Dynamic GitHub dataset link.(since it keeps changing all the time and it is different for different datasets)

   - `FolderName` – Folder in Data Lake. (eg : products folder)

   - `FileName` – CSV file name. (eg : product.csv)


Click Advanced in Set Properties

![image](https://github.com/user-attachments/assets/bc08623d-1dc3-473f-8125-d67be9cefb25)

Click add dynamic content

![image](https://github.com/user-attachments/assets/8ccf6a65-deaf-4010-9d7e-e7ee60ccef36)


Click add parameters

![image](https://github.com/user-attachments/assets/f6bb0d8e-f443-4e2d-8d60-678eca35fa66)

![image](https://github.com/user-attachments/assets/07aebaa2-5c77-40f9-89ef-39292d11af81)

![image](https://github.com/user-attachments/assets/15fe3f2a-8e0a-43f4-8788-9bdfb04dd0e4)

![image](https://github.com/user-attachments/assets/415fb2e3-714d-4431-ab7f-046fe6ae779f)



Now for sink as well , do the same

![image](https://github.com/user-attachments/assets/07e321ee-27ba-4df1-b169-033ef70211c4)



In the file path , just add the container name ie bronze since its always same/static

![image](https://github.com/user-attachments/assets/d04e5a5e-5dc1-4ca3-9bd5-073932d65590)


Then add dynamic content for directory , add a new parameter

![image](https://github.com/user-attachments/assets/11b4f5d1-41af-4019-8dc8-e85fbb044b4a)

![image](https://github.com/user-attachments/assets/f738ce00-78b1-43a7-92ce-4b50c8c9ea82)

Similarly for file name add dynamic content , create a new parameter

![image](https://github.com/user-attachments/assets/28be3c45-5a86-454c-b12b-c8827c42f6f1)

![image](https://github.com/user-attachments/assets/a6f54925-7848-4c95-b527-59da92c00666)

![image](https://github.com/user-attachments/assets/b0a7e7e1-f500-4a54-a30c-06c71de1a196)

![image](https://github.com/user-attachments/assets/79fbacb7-072b-4a2b-a842-986f3c40ab67)

![image](https://github.com/user-attachments/assets/a2fb381c-3d90-4ab3-aea5-11c0207e7d75)

![image](https://github.com/user-attachments/assets/eb2362c7-b610-41f2-9f47-733a1dd0c958)


Drag and drop the for each activity

![image](https://github.com/user-attachments/assets/defda1e9-0cb6-4084-9fc3-4914b0ca75ed)

![image](https://github.com/user-attachments/assets/ba1dd983-38a7-4144-9abd-59c325a2e4e7)







2. Modify source and sink properties to accept **dynamic content**.


### 🔹 Using ForEach Activity

1. **Create a JSON file** containing dataset parameters.

![image](https://github.com/user-attachments/assets/fe3d2703-738a-43b1-b4e1-6a625cd86d4f)


2. **Upload JSON file** to Data Lake., for this create a new container in your data lake resource
![image](https://github.com/user-attachments/assets/7983d389-1911-4f40-923c-4fd129e93e72)

![image](https://github.com/user-attachments/assets/3d8f2488-8243-491d-ae02-161459bfc2d1)




3. **Use Lookup Activity** to read JSON


Now , inorder to pull this data into adf, we need to add a new activity called lookup (output of the data)

![image](https://github.com/user-attachments/assets/76f0110b-2c5b-4c61-a0ac-7b61ab597597)



Go to settings  -> source dataset -> add new -> data lake ->json format

![image](https://github.com/user-attachments/assets/031ea97c-9d3d-46dd-9a26-83a768b315be)

![image](https://github.com/user-attachments/assets/e0f5d1f9-c291-4163-ba31-89707fa039f5)

![image](https://github.com/user-attachments/assets/b7f4bf6c-a52f-4f8f-a670-406f17417431)


Uncheck the first row only box

![image](https://github.com/user-attachments/assets/216ae6e3-e65d-46d5-a3e5-a97bd720fb80)



Now i just want to see the output of the lookup , for thst i will need to deactivate the other 2 activities ie ForEachGit and DynamicCopy

![image](https://github.com/user-attachments/assets/46b09d95-1441-490b-a318-d4285b0f358d)

![image](https://github.com/user-attachments/assets/1d63a517-290d-48a7-9f5f-72479b0b4d4a)



Now run it , only lookup will run now

![image](https://github.com/user-attachments/assets/c5c86778-1107-4f48-9a7b-f836889d3619)



Click the exit button near the activity name lookupGit to view the output

![image](https://github.com/user-attachments/assets/7bb25086-b354-4bba-a49b-8edcd91ec007)



Click on add dynamic content in the forEachGit Activity

![image](https://github.com/user-attachments/assets/5fbb0d42-26c6-4cb6-addd-db14d2d72e7f)

![image](https://github.com/user-attachments/assets/78e72683-8e23-47cb-b9b8-42fc0b18a11b)


Click on lookupGit

![image](https://github.com/user-attachments/assets/ac8fff84-a676-4df7-8cf6-a732e4247dda)



Now in the output we need to access the value variable

![image](https://github.com/user-attachments/assets/fba5b76f-7e82-49a0-81ee-2a518edd7b3e)



Now we’re getting the array from the lookup activity into the foreach activity . Cut and copy the dynamiccopy activity . Click on the pencil symbol as shown down , which opens a new canvas , now paste the dynamiccopy activity there

![image](https://github.com/user-attachments/assets/f4b03069-2456-488e-85a7-f67320ce80f3)




Pate the activity here , then in source’s  value add dynamic content

![image](https://github.com/user-attachments/assets/6936fd0a-35bc-4a9c-bde7-d7715e1b40ad)

![image](https://github.com/user-attachments/assets/4aa4048a-d5bc-4050-92eb-6a7984c17996)




For source, we have 1 parameter that i relative url

![image](https://github.com/user-attachments/assets/80257a91-e444-48ef-aee6-a6e0a45e4fed)


Similarly for sink , add dynamic content , and add th parameters ie file name and folder name parameter

![image](https://github.com/user-attachments/assets/886c3fda-0790-4510-9e05-c1051965dc4f)


Then RUN

![image](https://github.com/user-attachments/assets/3be34bcc-f10b-47ab-b62e-0582a827bd99)




Now you can verify it , by going to bronze container in azure data lake

![image](https://github.com/user-attachments/assets/2960b4d4-dcaf-4db0-9387-b6e63e5c6399)



Now we’re done with building pipeline in ADF ,



---


## 🏗 Step 4: Data Transformation with Azure Databricks


### 🔹 Connecting Databricks with Data Lake

1. **Create an Azure Databricks workspace**.

![image](https://github.com/user-attachments/assets/700a16c8-357c-4cbd-b900-5a7a7bb45779)

![image](https://github.com/user-attachments/assets/5c753433-b5b1-4d1e-9bdb-653b138b3ca3)






Compute is a cluster , click compute -> create a compute

![image](https://github.com/user-attachments/assets/76f377b7-fd50-4800-976b-32adc691b8f7)

![image](https://github.com/user-attachments/assets/6c0726e0-a35e-4aa3-8351-c65e39cdc49c)

![image](https://github.com/user-attachments/assets/75972b24-ee99-4736-9100-7a818659e711)



Now basically , we’re going to ingest the data from bronze layer to Silver layer using databricks with some transformation

![image](https://github.com/user-attachments/assets/4318e597-fb81-4676-9e89-595bf1645d5b)

.

2. **Generate a secret key** for authentication.

We’ll see how do we connect the data from datalake to databricks , we need access-level permission .  We neeed to make the key that would connect both of them


![image](https://github.com/user-attachments/assets/ad36674f-e571-431a-9689-71ce5ca05b64)

![image](https://github.com/user-attachments/assets/ba128fe1-7d63-4169-a71f-dd66ff568e9e)

![image](https://github.com/user-attachments/assets/871a2113-27dc-4a17-b9c3-20627e15e608)

![image](https://github.com/user-attachments/assets/3e738c9c-73e0-47f1-9a02-2fbf9d2f0c9c)

![image](https://github.com/user-attachments/assets/7cbcda0c-2743-4282-b47b-730905b55dea)

![image](https://github.com/user-attachments/assets/38fbc2a2-bdda-4716-a8fa-6ab9ef296e3f)


Save the above information in a notepad

Now we need to create a secret key

![image](https://github.com/user-attachments/assets/90de14af-ab04-4328-b352-34f6883a3d54)

![image](https://github.com/user-attachments/assets/7da28521-28fc-40c9-80de-552d16baadee)


Copy the value and secret id and add it to your notepad

![image](https://github.com/user-attachments/assets/a5f80c2b-7b04-4068-8088-195d0a40452c)




3. **Configure access-level permissions** in Data Lake

Once create , now we need to manage access control in our data lake


![image](https://github.com/user-attachments/assets/589032b7-b3f1-4185-a8df-ade380702888)

![image](https://github.com/user-attachments/assets/ef1686d4-99bd-4ae9-b145-0138758f97b5)

![image](https://github.com/user-attachments/assets/696e8304-b92a-4a9a-9f70-88158e198c7f)

![image](https://github.com/user-attachments/assets/7b811b41-a942-4e87-b418-ecf4075e196e)



Now let’s launch our azure databricks workspace and create a new folder, then create a notebook

![image](https://github.com/user-attachments/assets/de8328aa-1145-4547-a97a-8ab83fce96e3)



4. **Use Microsoft Documentation Code Snippet** to establish connection.

Now we connect the azure data lake with the databricks , get the code snippet from microsoft documentation -  (step 7) - [https://learn.microsoft.com/en-us/azure/databricks/connect/storage/tutorial-azure-storage](https://www.google.com/url?q=https://learn.microsoft.com/en-us/azure/databricks/connect/storage/tutorial-azure-storage&sa=D&source=editors&ust=1742244078483379&usg=AOvVaw0scC9Osnb0amQBGpxXxb8S)

![image](https://github.com/user-attachments/assets/d76a0e4e-0b92-4484-bf13-8315cec11c24)





📌 *Screenshot: Configuring Secrets in Databricks*


### 🔹 Transforming Data (Bronze to Silver Layer)

1. **Read Bronze layer data** into a Databricks notebook.

![image](https://github.com/user-attachments/assets/47c2603a-ef28-4ed6-858c-3c48b0dc200b)


2. **Apply transformations** (data cleaning, filtering, aggregations).

3. **Convert data to Parquet format**.

4. **Write transformed data** to the **Silver container** in Data Lake. (refer code)




---


## 🏗 Step 5: Structuring Data with Azure Synapse Analytics


Now we go into the next phase ie gold layer using azure synapse analytics . create a resource of it


### 🔹 Setting Up Synapse Analytics

1. **Create an Azure Synapse Analytics workspace**.

While creating the azure synapse resource , we’ve to create a new storage acc( data lake storage) for this

![image](https://github.com/user-attachments/assets/cdbad83c-108d-43af-8f47-47f78602ccc5)

![image](https://github.com/user-attachments/assets/d1b9ca7c-ed15-442b-a03c-205d6f320d1a)

![image](https://github.com/user-attachments/assets/7691e6eb-dd44-42b4-9712-1d3525d9dcd3)





Once deployed , go to resource group
![image](https://github.com/user-attachments/assets/4d7de929-d18f-4257-bd9e-40e0d0dd2971)

![image](https://github.com/user-attachments/assets/61662b87-c93e-474c-a69d-70f7fa52e5e9)

![image](https://github.com/user-attachments/assets/c044ec50-0813-4b54-b23b-f85a0919af1f)

![image](https://github.com/user-attachments/assets/d6598b21-530d-47bf-bfed-211de465c7a0)



Azure synapse analytics is a unified platform

![image](https://github.com/user-attachments/assets/20b0c0ec-0e47-4e7c-a855-6992963e6335)


When you navigate to Integrate , you can see a similar ui as ADF which has similar features

![image](https://github.com/user-attachments/assets/3abf57b0-f772-4fc2-a25d-dc4f88d12364)




When you navigate to develop , you can perform any kind of scripts be it sql or spark/notebook (we won’t call it as databricks)

![image](https://github.com/user-attachments/assets/3cd30779-56e2-479d-be4a-17f3e206fed0)

![image](https://github.com/user-attachments/assets/a90b6b4c-06d0-48eb-9144-84309dcd0ccd)

When you navigate to data , you can add database be it sql (create tables , fact/dim tables , data warehouse) or external data

Lake database - in spark when you create the data it is called lakehouse(in databricks) or lake database (in synapse)

![image](https://github.com/user-attachments/assets/1b387233-6773-443f-bef6-87aded50c6dd)


Synapse can be connected with data lake without creating any key as we did before , since both of them are azure products. Synapse itself by default has credentials(System manage Identity/Managed Identity)



### 🔹 Connecting Synapse to Data Lake

1. Use **Managed Identity Authentication** to access Data Lake.

2. Configure **IAM role assignments** in Data Lake.

3. Provision a serverless SQL pool

Now go to your data lake in your resource group to IAM , to add role assignment

![image](https://github.com/user-attachments/assets/f566c11e-2172-4aec-af03-95e1b32e826b)
![image](https://github.com/user-attachments/assets/3b97dd54-af8b-42b5-97bf-647be7814426)

Now go to your synapse workspace and create a SQL script
![image](https://github.com/user-attachments/assets/cc1d9aa9-ed73-489f-920a-bde7e6b4c17e)

Now let’s create the sql db first
![image](https://github.com/user-attachments/assets/0f81e8b2-a77e-428a-a314-b5d0e9ecb133)


Now we’ll choose serverless , since our data resides in the lakehouse and not physically in any database . cost is less when you store data in lakehouse .
![image](https://github.com/user-attachments/assets/877f1144-35c7-4210-bd17-2fc7918efe55)


Now got to your sql script , under use database dropdown you’ve option fot your newly created db ie awdatabase
![image](https://github.com/user-attachments/assets/171710a8-5ea2-45c2-8845-2b9af8074dc6)


Provide access to yourself in data lakehouse storage , to access the data , IAM -> ADD ROLE ->  search storage blob data contributor
![image](https://github.com/user-attachments/assets/4d85cf4e-4fa5-4220-9e4b-a1a32c961197)

### 🔹 Querying Data in Synapse


1. Use `OPENROWSET` to read Parquet data from **Silver Layer**.

OPENROWSET function in our sql script
To get the link of your data file go to your datalake storage -> silver
![image](https://github.com/user-attachments/assets/d547afc4-1c17-4434-9cc4-0504a1a03a74)
![image](https://github.com/user-attachments/assets/7e34b73d-423c-4011-84d2-a717d3584173)


We need not copy the entire url , only till folder name is required
![image](https://github.com/user-attachments/assets/a8941e3d-23f3-45bf-8a9e-0e81c0d6d9f6)


Here in the link it says blob.core , but we’re using datalake , so we’ve to change it to dfs (data file storage)
![image](https://github.com/user-attachments/assets/c8c79f94-2604-485b-a419-983849beb5d1)

With the help openrowset function you can return the parquet data asa tabular format
![image](https://github.com/user-attachments/assets/5a06e4cd-d2ac-4cfd-a2d7-f40046f68c98)
![image](https://github.com/user-attachments/assets/1e498012-7904-4954-ae5c-1660437a6324)

2.. Create **views** for frequently accessed datasets.

Similarly create views for all the datafiles
![image](https://github.com/user-attachments/assets/4609d44e-6196-419a-b40d-aace51626622)
![image](https://github.com/user-attachments/assets/cb9ca0aa-1dff-4527-b5a4-1a81c430a36f)


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
![image](https://github.com/user-attachments/assets/11bb2499-ab22-4c31-9546-980c0c7a614a)

Firstly we’ll have to create a master key
![image](https://github.com/user-attachments/assets/d30e9699-cbf7-4250-926a-f00d19914429)

Create database scoped credential ( from microsoft documentation refer the code)

![image](https://github.com/user-attachments/assets/71b0aafe-2533-4acd-a6d5-50b8ec93e17b)

Now let’s create the external datasource , we’ll have 2 creat 2 of them since we need to read data from silver , and push data to gold
![image](https://github.com/user-attachments/assets/cc0f5927-cf65-4b24-8451-c336e0d96a58)
![image](https://github.com/user-attachments/assets/0820ef3b-74c8-45f7-9beb-aace6816924f)
![image](https://github.com/user-attachments/assets/0df3494c-773a-444e-9a89-4365e2243ad5)




Now let’s go to 3rd step ie create external file format
![image](https://github.com/user-attachments/assets/9ff5eccd-71be-4110-8b5c-ac442e8840a2)




Now that we’ve created a view on silver , we’ll push the data from silver to gold to crete an external table using CETAS (Create External Table As)

![image](https://github.com/user-attachments/assets/a6e68f4d-9ec1-4cec-8d18-2ae36ac48d26)
![image](https://github.com/user-attachments/assets/176b5e72-a7fe-4e96-a687-08b197e7e24b)
![image](https://github.com/user-attachments/assets/89ab4802-83b0-45f5-8588-b16d7b7ad198)
![image](https://github.com/user-attachments/assets/9820e96c-d3c0-4e36-872b-520cc96c5666)









When we create External table , it is actually being stored in our datalake

![image](https://github.com/user-attachments/assets/f7518fd5-d7e9-4cde-afd4-983cb9bb3fa8)
![image](https://github.com/user-attachments/assets/6b1e3e23-207f-4abd-b265-654003d89b1a)









---


## 🏗 Step 6: Visualizing Data with Power BI


### 🔹 Connecting Power BI to Synapse

1. Retrieve **SQL Endpoint** from Synapse.

Now we got to establish conncection between synapse and PowerBI, For that we require sql endpoints
![image](https://github.com/user-attachments/assets/727d39ff-3830-429b-9e99-9a35b2d6aa25)
![image](https://github.com/user-attachments/assets/247e5f65-639a-4fef-a502-7ac46a449bfc)






Now copy that endpoint and go to powerbi -> get data


2. Open **Power BI → Get Data**.
![image](https://github.com/user-attachments/assets/87503706-ae40-4afb-886a-5c5e45ff49ef)





3. Paste the **SQL Endpoint** to establish a connection.

Paste the sql endpoint
![image](https://github.com/user-attachments/assets/24393728-ac7d-4a96-9420-94adb31bebda)
![image](https://github.com/user-attachments/assets/1d8cf370-0c12-4195-8343-8663af2e886b)






4. Build **dashboards and reports**.
![image](https://github.com/user-attachments/assets/9cf495cd-1e64-40fe-b26c-139fac49f737)





---



