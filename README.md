# 🚶 City of Melbourne Pedestrian Analytics Pipeline

> **Course:** BUS5001 – Cloud Platforms and Analytics | La Trobe University  
> **Tools:** Azure Data Factory · Azure Blob Storage · Azure SQL Database · Power BI · Azure Logic Apps  
> **Author:** Rahul Narayan Bale | Student ID: 22380204

---

## 📌 Project Overview

This project builds a fully automated, end-to-end cloud data pipeline to ingest, process, and analyse real-time pedestrian sensor data from the **City of Melbourne Open Data API**.

The goal is to help city planners understand pedestrian movement patterns — identifying peak hours, high-traffic hotspots, and total foot traffic — using a scalable cloud architecture on **Microsoft Azure**.

---

## 🏗️ Architecture Overview

```
City of Melbourne API
        │
        ▼
Azure Data Factory (ADF)
[pl_ingest_pedestrian_raw]
        │
        ├──► Azure Blob Storage (Raw Data)
        │         ds_blob_raw_pedestrian
        │
        ▼
  Data Transformation (ADF)
  - Clean (remove nulls & duplicates)
  - Derive columns (hour, date from timestamp)
  - Aggregate (minute → hourly counts per sensor)
  - Join with sensor metadata (location, street name)
  - Calculate peak counts per sensor
        │
        ▼
Azure SQL Database
  - dbo.table1           → Hourly pedestrian summaries
  - dbo.TableForPeak     → Peak hour counts per sensor
        │
        ▼
Power BI Dashboard
[4-panel interactive report]
        │
        ▼
Azure Logic Apps
[Automated CSV ingestion + Daily email summary]
```

---

## 📊 Key Results

| Metric | Value |
|--------|-------|
| Total pedestrian movements recorded | **574 million** |
| Data ingestion frequency | Every **15 minutes** |
| Azure services used | **4** (ADF, Blob Storage, SQL DB, Logic Apps) |
| Dashboard panels | **4** |
| Peak morning hour | **8–9 AM** |
| Peak evening hour | **5–6 PM** |
| Top busiest sensor location | **Flinders Street / Southbank** |

---

## 🔧 Step-by-Step Process

### Step 1 — Data Ingestion
- Created a pipeline named **`pl_ingest_pedestrian_raw`** in Azure Data Factory
- Configured an **HTTP linked service** with anonymous authentication to connect to the City of Melbourne Open Data API
- Pipeline runs every **15 minutes** to simulate real-time data collection
- Raw data is stored in Azure Blob Storage as dataset **`ds_blob_raw_pedestrian`**

### Step 2 — Data Transformation
- **Cleaning:** Removed records with missing, null, or duplicate values
- **Derived Columns:** Extracted `hour` and `date` from the timestamp field
- **Aggregation:** Converted minute-level counts into **hourly summaries per sensor**
- **Join:** Merged pedestrian data with **sensor metadata** to attach street name, location, and area details
- **Peak Calculation:** Calculated maximum pedestrian count per sensor to find busiest times

### Step 3 — Data Loading
- Loaded transformed data into **Azure SQL Database** via linked service `AzureSqlDatabase1`
- Two tables created:
  - `dbo.table1` — hourly pedestrian summaries
  - `dbo.TableForPeak` — peak counts per sensor
- Used **AutoResolveIntegrationRuntime** as the compute environment

### Step 4 — Power BI Dashboard
Connected Power BI to Azure SQL Database and built 4 panels:

| Panel | Description |
|-------|-------------|
| Pedestrian Trend Over Time | Hourly movement by sensor showing 8–9 AM and 5–6 PM peaks |
| Pedestrian Density Map | Hotspot map using latitude/longitude — CBD, Flinders St, Southbank |
| Top 5 Busiest Sensors | Bar chart: SouthB_T, Swa31_T, ElFi_T, 261Will_T, QVN_T |
| Total Pedestrian Volume | KPI card showing **574 million** total movements |

### Step 5 — Logic Apps Automation
- **Logic App 1:** Triggered by incoming emails with CSV attachments → saves files to the `unprocessed-files` Blob container
- **Logic App 2:** Reads files from `unprocessed-files`, processes row counts, moves files to `processed-files` container, and sends a **daily engagement summary email**

---

## 📁 Repository Structure

```
pedestrian-analytics-pipeline/
│
├── README.md                          ← You are here
│
├── screenshots/
│   ├── 01_resource_group.png          ← Azure resource group overview
│   ├── 02_storage_account.png         ← Storage account setup
│   ├── 03_containers.png              ← Blob containers (unprocessed + processed)
│   ├── 04_adf_pipeline_overview.png   ← ADF pipeline canvas
│   ├── 05_linked_service_http.png     ← HTTP linked service config
│   ├── 06_data_transformation.png     ← Data flow (clean, aggregate, join)
│   ├── 07_sql_database.png            ← Azure SQL DB tables
│   ├── 08_pipeline_runs.png           ← Successful pipeline run history
│   ├── 09_powerbi_dashboard.png       ← Full Power BI dashboard
│   ├── 10_powerbi_trend.png           ← Pedestrian trend panel
│   ├── 11_powerbi_map.png             ← Density map panel
│   ├── 12_powerbi_sensors.png         ← Top 5 busiest sensors panel
│   ├── 13_logic_app1.png              ← Logic App 1 (email ingestion)
│   ├── 14_logic_app2.png              ← Logic App 2 (processing + email)
│   └── 15_summary_email.png          ← Daily engagement summary email received
│
├── docs/
│   ├── pipeline_sop.md                ← Step-by-step SOP write-up
│   └── findings_and_recommendations.md ← Key insights and recommendations
│
└── report/
    └── BUS5001_Assignment2_Report.pdf  ← Full assignment report (optional)
```

---

## 📸 Screenshots

### Azure Data Factory — Pipeline Overview
![ADF Pipeline](screenshots/04_adf_pipeline_overview.png)

### Power BI Dashboard
![Power BI Dashboard](screenshots/09_powerbi_dashboard.png)

### Pipeline Run History (Successful)
![Pipeline Runs](screenshots/08_pipeline_runs.png)

### Logic App — Daily Email Summary
![Summary Email](screenshots/15_summary_email.png)

---

## 💡 Key Findings

- **Peak pedestrian hours** are 8–9 AM and 5–6 PM, matching morning and evening commute periods
- **Busiest locations** are Flinders Street Station, Swanston Street, Southbank, and the RMIT precinct
- **574 million** total pedestrian movements were recorded across all sensors
- Midday activity is moderate, with a steady decline after 7 PM

## ✅ Recommendations

- Expand pedestrian pathways and crosswalk capacity in high-traffic areas
- Implement adaptive traffic signal control during peak hours
- Maintain continuous 15-minute data collection for long-term trend forecasting
- Use dashboard insights to schedule maintenance and safety upgrades in busy zones

---

## 🛠️ Technologies Used

| Technology | Purpose |
|------------|---------|
| Azure Data Factory | Pipeline orchestration and data transformation |
| Azure Blob Storage | Raw and processed file storage |
| Azure SQL Database | Structured storage for reporting |
| Power BI | Interactive dashboard and visualisation |
| Azure Logic Apps | Workflow automation and email reporting |
| AutoResolveIntegrationRuntime | Compute environment for ADF pipeline execution |
| City of Melbourne Open Data API | Source of real-time pedestrian sensor data |

---

## 👤 About

**Rahul Narayan Bale**  
Master of Business Analytics — La Trobe University, Melbourne  
📧 rahulbale2804@gmail.com  
🔗 [GitHub Profile](https://github.com/RahulBale28)

---

> *This project was completed as part of BUS5001 – Cloud Platforms and Analytics at La Trobe University.*
