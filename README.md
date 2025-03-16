# Adventure-Works-Data-Engineering-Project


# Azure Data Engineering Project

This project demonstrates a complete **ETL pipeline** using Azure services, including **Azure Data Lake, Azure Data Factory (ADF), Databricks, and Synapse Analytics**. The goal is to extract, transform, and load data from GitHub into a structured data warehouse, leveraging **bronze, silver, and gold layers** for efficient data processing.

## 📌 Project Overview

### 🔹 Azure Resources Used
1. **Resource Group** – Organizes all the services under one group.
2. **Azure Data Lake Storage** – Stores raw, processed, and transformed data.
3. **Azure Data Factory (ADF)** – Handles data ingestion and movement.
4. **Azure Databricks** – Processes data with transformations.
5. **Azure Synapse Analytics** – Stores final structured data and integrates with Power BI.

---

## 🏗 Step 1: Setting Up Azure Data Lake Storage
- Created a **Storage Account** in Azure with hierarchical namespace enabled for better performance with big data workloads.
- Defined **three containers**:
  - 🟤 **Bronze Layer** – Stores raw, unprocessed data as ingested from the source.
  - ⚪ **Silver Layer** – Contains cleaned, formatted, and enriched data after processing.
  - 🟡 **Gold Layer** – Stores aggregated and structured data optimized for analytics and reporting.
- Configured **role-based access control (RBAC)** to allow ADF and Databricks to interact securely.

📌 *Screenshot: Data Lake Storage Configuration*

---

## 🏗 Step 2: Configuring Azure Data Factory (ADF)

### 🔹 Creating the Data Pipeline
1. Open **ADF Studio** and navigate to the **Author** tab.
2. Create a **new pipeline** and give it a meaningful name.
3. Under **Activities → Move & Transform**, drag and drop the **Copy Data** activity.
4. Configure the **Source (Data Source)** and **Sink (Destination)** to define the data flow.

📌 *Screenshot: ADF Pipeline Overview*

### 🔹 Setting Up Linked Services
- **GitHub Data Source**: 
  - Used an **HTTP connection** to fetch data from GitHub.
  - Dataset Link: [AdventureWorks Sales Data](https://raw.githubusercontent.com/anshlambaoldgit/Adventure-Works-Data-Engineering-Project/refs/heads/main/Data/AdventureWorks_Sales_2015.csv)
  - Base URL: `https://raw.githubusercontent.com/`
- **Azure Data Lake as Sink**: Configured a **linked service** to store data in the Bronze layer.

📌 *Screenshot: Creating Linked Services in ADF*

### 🔹 Running the Pipeline
- Click **Debug** to test the pipeline execution.
- Once successful, the CSV file appears in the **Bronze container**.
- Click **Publish** to save the pipeline.

📌 *Screenshot: Data Successfully Copied to Data Lake*

---

## 🏗 Step 3: Building Dynamic Pipelines in ADF
Instead of manually setting up a copy activity for each dataset, **parameters and looping constructs** are used to make the pipeline dynamic and reusable.

### 🔹 Dynamic Pipeline Setup
1. Created a new pipeline with **three parameters**:
   - `RelativeLink` (dynamic GitHub link, varies for different datasets)
   - `FolderName` (e.g., `products_folder`)
   -
