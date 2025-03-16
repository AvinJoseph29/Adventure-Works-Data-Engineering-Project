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

📌 *Screenshot: Data Lake Storage Configuration*

---

## 🏗 Step 2: Configuring Azure Data Factory (ADF)

### 🔹 Creating the Data Pipeline
1. Open **ADF Studio** → **Author Tab** → Click **New Pipeline**.
2. Under **Activities → Move & Transform**, drag and drop the **Copy Data** activity.
3. **Configure Source and Sink**:
   - **Source:** GitHub dataset (CSV file).
   - **Sink:** Azure Data Lake (Bronze Container).

📌 *Screenshot: ADF Pipeline Overview*

### 🔹 Setting Up Linked Services
1. **Create a Linked Service for GitHub**:
   - Choose **HTTP Connector**.
   - Provide **Base URL**: `https://raw.githubusercontent.com/`
   - Use relative path: `anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv`
2. **Create a Linked Service for Data Lake**:
   - Choose **Azure Data Lake Storage Gen2**.
   - Provide authentication details.

📌 *Screenshot: Creating Linked Services in ADF*

### 🔹 Running the Pipeline
1. Click **Debug** to test pipeline execution.
2. Verify the file in **Bronze container** in Data Lake.
3. Click **Publish** to save the pipeline.

📌 *Screenshot: Data Successfully Copied to Data Lake*

---

## 🏗 Step 3: Building Dynamic Pipelines in ADF

### 🔹 Configuring Parameters
1. Define **three parameters**:
   - `RelativeLink` – Dynamic GitHub dataset link.
   - `FolderName` – Folder in Data Lake.
   - `FileName` – CSV file name.
2. Modify source and sink properties to accept **dynamic content**.

### 🔹 Using ForEach Activity
1. **Create a JSON file** containing dataset parameters.
2. **Upload JSON file** to Data Lake.
3. **Use Lookup Activity** to read JSON and pass it to **ForEach Activity**.
4. **Copy Data Activity** dynamically processes multiple datasets.

📌 *Screenshot: Dynamic Pipeline Execution in ADF*

---

## 🏗 Step 4: Data Transformation with Azure Databricks

### 🔹 Connecting Databricks with Data Lake
1. **Create an Azure Databricks workspace**.
2. **Deploy a compute cluster**.
3. **Configure access-level permissions** in Data Lake.
4. **Generate a secret key** for authentication.
5. **Use Microsoft Documentation Code Snippet** to establish connection.

📌 *Screenshot: Configuring Secrets in Databricks*

### 🔹 Transforming Data (Bronze to Silver Layer)
1. **Read Bronze layer data** into a Databricks notebook.
2. **Apply transformations** (data cleaning, filtering, aggregations).
3. **Convert data to Parquet format**.
4. **Write transformed data** to the **Silver container** in Data Lake.

📌 *Screenshot: Transformed Data in Silver Layer*

---

## 🏗 Step 5: Structuring Data with Azure Synapse Analytics

### 🔹 Setting Up Synapse Analytics
1. **Create an Azure Synapse Analytics workspace**.
2. **Provision a serverless SQL pool**.

### 🔹 Connecting Synapse to Data Lake
1. Use **Managed Identity Authentication** to access Data Lake.
2. Configure **IAM role assignments** in Data Lake.

### 🔹 Querying Data in Synapse
1. **Create a SQL Database** (`awdatabase`).
2. Use `OPENROWSET` to read Parquet data from **Silver Layer**.
3. Create **views** for frequently accessed datasets.

📌 *Screenshot: Querying Data in Synapse SQL Pool*

### 🔹 Creating External Tables
1. **Create a Database Master Key**.
2. **Define Database Scoped Credentials**.
3. **Create External Data Sources** for Silver and Gold layers.
4. **Define External File Format** for Parquet.
5. **Move data from Silver to Gold using `CETAS`**.
6. Verify structured data in **Gold container**.

📌 *Screenshot: External Table Creation in Synapse*

---

## 🏗 Step 6: Visualizing Data with Power BI

### 🔹 Connecting Power BI to Synapse
1. Retrieve **SQL Endpoint** from Synapse.
2. Open **Power BI → Get Data**.
3. Paste the **SQL Endpoint** to establish a connection.
4. Build **dashboards and reports**.

📌 *Screenshot: Power BI Dashboard Connected to Synapse*

---

## 🎯 Conclusion
This project successfully implements an **end-to-end ETL pipeline** using Azure services. The structured approach ensures **scalable and efficient data processing**:
- **Raw data ingestion** from GitHub (**Bronze Layer**).
- **Data transformation** using Databricks (**Silver Layer**).
- **Data structuring & querying** in Synapse (**Gold Layer**).
- **Data visualization** in Power BI.

This setup is ideal for real-world **data engineering workflows** with automated ingestion, transformation, and reporting capabilities.
