
#  NYC Green Taxi Revenue Data Engineering Project

## 📌 Project Overview

This project demonstrates an **end-to-end Azure Data Engineering pipeline** designed to investigate an unexplained drop in **Green Taxi booking revenue in 2025**.

The objective is to collect raw taxi trip data from the **NYC Taxi & Limousine Commission**, build a scalable data pipeline using Azure services, transform and organize the data into different layers, and finally make the processed data available to **Data Analysts and Business Users through Power BI**.

The project follows the **Medallion Architecture**:

**Bronze → Silver → Gold**

---

# 🎯 Problem Statement

The company's business manager noticed an **unexplained drop in Green Taxi booking revenue during 2025**.

The manager asked the Data Engineering team:

> "We need reliable and structured taxi booking data so that Data Analysts and Business Users can investigate the reason behind the revenue decline."

To solve this problem, I designed and implemented an **end-to-end Azure data pipeline** that:

1. Fetches taxi trip data from the NYC Taxi & Limousine Commission website.
2. Stores the original/raw data in Azure Data Lake Storage.
3. Transforms and cleans the data using Azure Databricks.
4. Creates Delta Tables for the business-ready Gold layer.
5. Connects the Gold layer to Power BI for reporting and analysis.

---

# 🏗️ Architecture

The project uses the following architecture:

**NYC Taxi & Limousine Commission → Azure Data Factory → Azure Data Lake Storage (Bronze) → Azure Databricks → Azure Data Lake Storage (Silver) → Azure Databricks → Delta Tables (Gold) → Power BI**

### Architecture Diagram

> 📌 **PLACE THE ARCHITECTURE IMAGE HERE**
>
> Add the architecture image created for this project below this section.
>
> Recommended location in your repository:
>
> `ARCHITECTURE_DIAGRAM`

```text
![NYC Taxi Data Engineering Architecture](docs/architecture.png)
```

---

# 🛠️ Technologies Used

| Technology                          | Purpose                                   |
| ----------------------------------- | ----------------------------------------- |
| **NYC Taxi & Limousine Commission** | Source of taxi trip datasets              |
| **Azure Data Factory**              | Data ingestion and pipeline orchestration |
| **Azure Data Lake Storage Gen2**    | Storage for Bronze, Silver and Gold data  |
| **Azure Databricks**                | Data transformation and processing        |
| **Apache Spark / PySpark**          | Distributed data processing               |
| **Delta Lake**                      | Creation of reliable Delta Tables         |
| **Power BI**                        | Data visualization and business reporting |

---

# 🔄 End-to-End Data Flow

The complete pipeline consists of five major steps:

```text
NYC TLC Website
       ↓
Azure Data Factory
       ↓
Bronze Container
       ↓
Azure Databricks
       ↓
Silver Container
       ↓
Azure Databricks
       ↓
Gold Delta Tables
       ↓
Power BI
       ↓
Data Analysts / Business Users
```

---

# 1️⃣ Data Ingestion Using Azure Data Factory

## Objective

The first step is to collect the raw taxi trip data from the **NYC Taxi & Limousine Commission website**.

The data contains information related to taxi trips and can be used to analyze factors such as:

* Trip dates
* Pickup and drop-off locations
* Passenger information
* Trip distance
* Fare amount
* Payment information
* Tip amount
* Total amount
* Other trip-related attributes

## Azure Data Factory

**Azure Data Factory (ADF)** is used as the data ingestion and orchestration service.

The pipeline connects to the publicly available NYC Taxi dataset and copies the required data into Azure Data Lake Storage.

### Flow

```text
NYC TLC Website
       ↓
Azure Data Factory
       ↓
Azure Data Lake Storage
       ↓
Bronze Container
```

ADF is responsible for:

* Connecting to the source
* Fetching the data
* Copying the data
* Managing the ingestion pipeline
* Orchestrating the data movement

---

# 2️⃣ Bronze Layer – Raw Data

After ADF fetches the data, it is stored in the **Bronze container** of Azure Data Lake Storage.

The Bronze layer contains the **raw/original data**.

The objective of this layer is to preserve the source data before performing transformations.

### Example

```text
Azure Data Lake Storage
│
└── Bronze
    │
    ├── green_taxi/
    │   ├── 2025-01.parquet
    │   ├── 2025-02.parquet
    │   ├── 2025-03.parquet
    │   └── ...
    │
    └── other_raw_data/
```

### Why Bronze Layer?

The Bronze layer provides:

* Raw data preservation
* Historical data storage
* Ability to reprocess data
* Separation between raw and processed data
* Traceability back to the original source

**Important:** No major business transformations are performed in the Bronze layer.

---

# 3️⃣ Silver Layer – Data Transformation

Once the raw data is stored in Bronze, the next step is to process it.

For this step, **Azure Databricks** is used.

Azure Databricks reads the raw data from the Bronze container and performs the required transformations using **PySpark/Spark**.

### Flow

```text
Bronze Container
       ↓
Azure Databricks Notebook
       ↓
Read Raw Data
       ↓
Data Cleaning
       ↓
Data Transformation
       ↓
Data Quality Checks
       ↓
Silver Container
```

## Databricks Notebook

A Databricks notebook is created to read the Bronze data.

Example:

```python
df = spark.read.parquet(bronze_path)
```

The data is then processed using PySpark.

---

## Typical Transformations

The Silver layer can contain transformations such as:

### Data Cleaning

* Handling null values
* Removing duplicate records
* Handling invalid records
* Correcting data types

### Data Transformation

* Converting timestamp columns
* Creating date-related columns
* Standardizing column names
* Calculating derived columns
* Formatting categorical fields

### Data Quality

Data quality checks can be performed to identify:

* Null values
* Invalid values
* Duplicate records
* Incorrect data types
* Unexpected values

After transformation, the processed data is stored in the **Silver container**.

---

# 4️⃣ Gold Layer – Delta Tables

After creating the Silver layer, the data is read again using **Azure Databricks**.

The purpose of the Gold layer is to create **business-ready datasets** that can be efficiently consumed by analysts and reporting tools.

### Flow

```text
Silver Container
       ↓
Azure Databricks
       ↓
Business Transformations
       ↓
Delta Tables
       ↓
Gold Container
```

---

## Why Delta Tables?

Delta Lake provides capabilities such as:

* ACID transactions
* Schema enforcement
* Schema evolution
* Reliable data updates
* Better data management
* Version history/time travel
* Reliable processing of data

The transformed Silver data is used to create Delta Tables in the Gold layer.

---

# 🥇 Gold Layer Design

The Gold layer contains data specifically prepared for analytical workloads.

For example:

```text
Gold
│
├── fact_green_taxi_trips
│
├── dim_date
│
├── dim_location
│
└── other_business_tables
```

The exact table structure can be designed according to the analytical requirements.

The Gold layer should contain data that is:

* Clean
* Structured
* Business-ready
* Optimized for analytical queries
* Easy for BI users to understand

---

# 5️⃣ Power BI – Data Visualization

The final step is to make the Gold-layer data available to business users.

**Power BI** is connected to the Gold data.

### Flow

```text
Gold Delta Tables
       ↓
Power BI
       ↓
Reports
       ↓
Dashboards
       ↓
Business Users
```

Power BI can be used to create dashboards that help investigate the revenue decline.

---

# 📊 Business Analysis

The main business question is:

> **Why did Green Taxi booking revenue decrease in 2025?**

The Power BI dashboard can help business users investigate different factors behind the revenue change.

For example:

### Revenue Analysis

* Total revenue
* Revenue by month
* Revenue by year
* Revenue growth/decline
* Average revenue per trip

### Trip Analysis

* Total number of trips
* Trips by month
* Average trip distance
* Average fare
* Average trip duration

### Location Analysis

* Pickup locations
* Drop-off locations
* Revenue by location
* Number of trips by location

### Customer/Payment Analysis

* Payment types
* Tip amounts
* Average fare
* Passenger-related metrics

These metrics can help analysts identify patterns responsible for the revenue decline.

---

# 🏛️ Medallion Architecture

This project follows the **Medallion Architecture**.

```text
                 MEDALLION ARCHITECTURE

                    RAW DATA
                       │
                       ▼
              ┌─────────────────┐
              │     BRONZE      │
              │   Raw Dataset   │
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │     SILVER      │
              │ Cleaned &       │
              │ Transformed Data│
              └────────┬────────┘
                       │
                       ▼
              ┌─────────────────┐
              │      GOLD       │
              │ Business-Ready  │
              │  Delta Tables   │
              └────────┬────────┘
                       │
                       ▼
                  ┌─────────┐
                  │ POWER BI│
                  └─────────┘
```

---

# 🔵 Bronze Layer

**Purpose:** Store raw source data.

**Technology:**

* Azure Data Lake Storage Gen2
* Azure Data Factory

**Data:** Raw NYC Taxi data.

---

# ⚪ Silver Layer

**Purpose:** Clean and transform raw data.

**Technology:**

* Azure Databricks
* Apache Spark
* PySpark
* Azure Data Lake Storage

**Data:** Cleaned and transformed taxi data.

---

# 🟡 Gold Layer

**Purpose:** Store business-ready analytical data.

**Technology:**

* Azure Databricks
* Delta Lake
* Azure Data Lake Storage

**Data:** Delta Tables optimized for analytics.

---

# 🟣 Reporting Layer

**Purpose:** Provide insights to business users.

**Technology:**

* Power BI

**Users:**

* Data Analysts
* Business Analysts
* Business Managers
* Decision Makers

---

# 🔐 Data Flow Summary

| Stage         | Service            | Action                       | Output               |
| ------------- | ------------------ | ---------------------------- | -------------------- |
| Source        | NYC TLC Website    | Provide taxi data            | Raw dataset          |
| Ingestion     | Azure Data Factory | Fetch/copy data              | Bronze data          |
| Storage       | Azure Data Lake    | Store raw data               | Bronze container     |
| Processing    | Azure Databricks   | Clean & transform            | Silver data          |
| Storage       | Azure Data Lake    | Store transformed data       | Silver container     |
| Processing    | Azure Databricks   | Create business-ready tables | Delta Tables         |
| Storage       | Azure Data Lake    | Store Gold data              | Gold container       |
| Visualization | Power BI           | Analyze and visualize        | Reports & Dashboards |

---

# 🔁 Complete Pipeline

The complete project can be summarized as:

```text
┌──────────────────────────────┐
│ NYC Taxi & Limousine         │
│ Commission Website            │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Azure Data Factory            │
│ Data Ingestion                │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Azure Data Lake Storage       │
│                              │
│       BRONZE LAYER           │
│       Raw Data               │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Azure Databricks              │
│ PySpark / Spark               │
│ Data Transformation           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Azure Data Lake Storage       │
│                              │
│       SILVER LAYER           │
│    Cleaned Data              │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Azure Databricks              │
│ Business Transformations      │
│ Delta Table Creation          │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Azure Data Lake Storage       │
│                              │
│       GOLD LAYER             │
│       Delta Tables           │
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│ Power BI                     │
│ Reports & Dashboards         │
└──────────────┬───────────────┘
               │
               ▼
       Business Users
       & Data Analysts
```

---


# 🚀 Key Data Engineering Concepts Demonstrated

This project demonstrates practical knowledge of:

* Azure Data Factory
* Azure Data Lake Storage Gen2
* Azure Databricks
* Apache Spark
* PySpark
* Data ingestion
* ETL/ELT pipelines
* Medallion Architecture
* Bronze/Silver/Gold architecture
* Data transformation
* Data cleansing
* Data quality
* Delta Lake
* Delta Tables
* Data Lake architecture
* Business-ready data modeling
* Power BI integration
* Data analytics

---

# 🎯 Project Outcome

The final solution provides the company with a structured data platform where:

**Raw data** is collected from the NYC Taxi & Limousine Commission.

↓

**Bronze layer** preserves the original data.

↓

**Silver layer** contains cleaned and transformed data.

↓

**Gold layer** contains business-ready Delta Tables.

↓

**Power BI** provides reports and dashboards.

↓

**Business users and Data Analysts** can investigate the factors contributing to the Green Taxi revenue decline in 2025.

---

# 💡 What This Project Demonstrates

This project demonstrates how a Data Engineer can convert a business problem into a complete data solution.

Instead of directly giving raw data to analysts, the pipeline creates different layers for different purposes:

```text
Raw Data
   ↓
Bronze
   ↓
Cleaned Data
   ↓
Silver
   ↓
Business Data
   ↓
Gold
   ↓
Analytics
   ↓
Power BI
```

This architecture makes the data platform **organized, scalable, maintainable, and suitable for analytical workloads**.

---

# 👨‍💻 Author

**Prashant More**

Azure Data Engineering Project

**Technologies:** Azure Data Factory | Azure Data Lake Storage Gen2 | Azure Databricks | PySpark | Delta Lake | Power BI
