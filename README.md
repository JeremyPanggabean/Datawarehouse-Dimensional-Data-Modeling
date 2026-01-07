# Data Warehouse Dimensional Data Modeling on Databricks

## Overview

This project demonstrates an **Dimensional Data Warehouse implementation** using **Databricks**, designed with **Star Schema modeling**, **Change Data Capture (CDC)**, and **Slowly Changing Dimension (SCD) Type 1 (UPSERT)** strategies.


## 🎯 Objectives

* Implement **dimensional data modeling** using a **Star Schema**
* Handle **incremental data changes** using **Change Data Capture (CDC)**
* Apply **Slowly Changing Dimension (SCD) Type 1** for dimension updates
* Build an **analytics-ready data warehouse** optimized for BI and reporting

---

## 🏗️ Architecture

**Source Systems**
Transactional databases / operational systems

⬇️ (CDC / Incremental Load)

**Bronze Layer**

* Raw data ingestion
* Schema preserved from source

⬇️

**Silver Layer**

* Cleaned & standardized data
* Deduplicated records
* CDC logic applied

⬇️

**Gold Layer (Data Warehouse)**

* Star Schema tables
* Fact & Dimension tables
* Optimized for analytical queries

---

## ⭐ Star Schema Design

### Fact Table

* `fact_sales`

  * transaction_id
  * amount
  * transaction_key
  * customer_key
  * account_key
  * product_key
  * branch_key
  * datetime_key

### Dimension Tables

* `dim_transactions`
* `dim_customers`
* `dim_account`
* `dim_product`
* `dim_branch`
* `dim_datetime`

Each **fact table** stores business metrics, while **dimension tables** provide descriptive context.

This structure ensures:

* Simpler SQL queries
* Better BI performance
* Clear business logic separation


---

## 🔄 Change Data Capture (CDC)

CDC is used to **detect and process data changes** (INSERT, UPDATE, DELETE) from source systems without full reloads.

### CDC Strategy

* Source provides `operation_type` or `last_updated_timestamp`
* Incremental loads processed in the Silver layer
* Only changed records are propagated downstream

### Benefits

* Reduced processing cost
* Near real-time data freshness
* Scalable for large datasets

---

## 🕒 Slowly Changing Dimension (SCD Type 1)

This project applies **SCD Type 1**, where **dimension records are overwritten** when changes occur.

### Why SCD Type 1?

* No need to track historical changes
* Latest value is always the source of truth
* Simpler storage and query logic

### Implementation (UPSERT / MERGE)

```sql
MERGE INTO dim_customers AS target
USING staging_customers AS source
ON target.customer_id = source.customer_id
WHEN MATCHED THEN
  UPDATE SET *
WHEN NOT MATCHED THEN
  INSERT *
```

Feel free to fork, explore, and adapt 🚀
