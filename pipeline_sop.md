# Pipeline SOP — City of Melbourne Pedestrian Analytics

## Overview
This document outlines the Standard Operating Procedure (SOP) for the cloud-based pedestrian data pipeline built in Microsoft Azure.

---

## Step 1 — Create Azure Resources
1. Sign in to the **Azure Portal**
2. Create a **Resource Group** (e.g. `BUS5001-Week5`)
3. Create a **Storage Account** named `storageaccount22380204` in the `australiaeast` region
4. Create two **Blob Containers**:
   - `unprocessed-files` — stores raw CSV attachments
   - `processed-files` — stores validated, transformed data

---

## Step 2 — Set Up Azure Data Factory
1. Create an **Azure Data Factory** resource in the same resource group
2. Open **ADF Studio** and create a new pipeline named `pl_ingest_pedestrian_raw`
3. Add a **Linked Service** of type HTTP (anonymous authentication) pointing to the City of Melbourne Open Data API endpoint
4. Create dataset `ds_blob_raw_pedestrian` pointing to the Blob Storage raw container
5. Set pipeline trigger to run every **15 minutes**

---

## Step 3 — Build the Data Transformation Flow
Inside ADF Data Flow, add the following steps in order:

| Step | Action | Description |
|------|--------|-------------|
| 1 | Source | Read raw pedestrian data from Blob Storage |
| 2 | Filter/Clean | Remove rows with null, missing, or duplicate values |
| 3 | Derived Column | Extract `hour` and `date` from the timestamp field |
| 4 | Aggregate | Convert minute-level counts to hourly summaries per sensor |
| 5 | Join | Merge with sensor metadata table (adds street name, area, location) |
| 6 | Peak Calc | Calculate maximum count per sensor (peak hours) |
| 7 | Sink | Write to Azure SQL Database |

---

## Step 4 — Load into Azure SQL Database
1. Create a **Linked Service** for Azure SQL Database (`AzureSqlDatabase1`)
2. Create two sink tables:
   - `dbo.table1` — hourly pedestrian summaries
   - `dbo.TableForPeak` — peak count per sensor
3. Use **AutoResolveIntegrationRuntime** as the compute environment
4. Run the pipeline and verify in **Monitor → Pipeline Runs**

---

## Step 5 — Connect Power BI
1. Open Power BI Desktop
2. Connect to **Azure SQL Database** using the server and database credentials
3. Load `dbo.table1` and `dbo.TableForPeak`
4. Build 4 dashboard panels:
   - Line chart: Pedestrian trend over time (by hour)
   - Map: Pedestrian density by latitude/longitude
   - Bar chart: Top 5 busiest sensors
   - KPI card: Total pedestrian volume

---

## Step 6 — Set Up Logic Apps
### Logic App 1 — Email Ingestion
- Trigger: When a new email arrives with CSV attachment
- Action: Save attachment to `unprocessed-files` Blob container

### Logic App 2 — Daily Processing
- Trigger: Recurrence (daily)
- Actions:
  1. List blobs in `unprocessed-files`
  2. Get blob content for each file
  3. Count rows and accumulate total
  4. Move file to `processed-files` container
  5. Delete from `unprocessed-files`
  6. Send summary email with total engagement count

---

## Verification Checklist
- [ ] Pipeline runs every 15 minutes without errors
- [ ] Blob Storage shows new files in the raw container
- [ ] SQL Database tables are populated with hourly data
- [ ] Power BI dashboard refreshes and shows correct numbers
- [ ] Logic App run history shows successful executions
- [ ] Summary email received with correct engagement count
