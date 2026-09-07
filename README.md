
# 🛒 Instacart Medallion ETL Pipeline with Delta Live Tables (DLT)

![Databricks](https://img.shields.io/badge/Databricks-FF3621?style=for-the-badge&logo=Databricks&logoColor=white)
![Apache Spark](https://img.shields.io/badge/Apache_Spark-E25A1C?style=for-the-badge&logo=apachespark&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-4479A1?style=for-the-badge&logo=postgresql&logoColor=white)
![Git](https://img.shields.io/badge/Git-F05032?style=for-the-badge&logo=git&logoColor=white)

## 📌 Project Overview
This project implements an end-to-end Data Engineering pipeline for the **Instacart Market Basket Dataset** using **Databricks Delta Live Tables (DLT)**. The architecture follows the **Medallion Architecture** pattern (Bronze ➔ Silver ➔ Gold) to transform raw data into a structured **Star Schema** optimized for BI and business analytics.

---

## 🏗️ Architecture & Pipeline Flow (DAG)

Here is the DAG execution graph of the Delta Live Tables pipeline running in Databricks:

<!-- 📸 حط مسار أو رابط الصورة بتاعتك هنا بدل المسار الافتراضي -->
![Delta Live Tables Pipeline DAG]<img width="1000" height="650" alt="Capture" src="https://github.com/user-attachments/assets/d1cccd7e-b3c9-4ebc-a889-a7c215df8215" />


---

## 🛠️ Tech Stack & Technologies Used

* **Orchestration & Framework:** Databricks Delta Live Tables (DLT), Auto Loader (`cloud_files`)
* **Processing Engines:** Apache Spark (PySpark & Spark SQL)
* **Storage & Catalog:** Databricks Unity Catalog, Volumes, Delta Lake
* **Languages:** Python, SQL
* **Data Modeling:** Dimensional Modeling (Star Schema - Fact & Dimension Tables)
* **Version Control:** Git & GitHub

---

## 📐 Data Lakehouse Layers (Medallion Architecture)

### 🥉 1. Bronze Layer (Raw Ingestion)
- Ingests raw CSV files incrementally using Databricks **Auto Loader (`cloud_files`)**.
- Handles automatic schema inference and evolution.
- Stores raw data from departments, aisles, products, orders, and order products (`prior` & `train`).

### 🥈 2. Silver Layer (Cleansing & Standardization)
- Cleanses, standardizes, and enforces correct data types across tables.
- Establishes pipeline lineage using `LIVE.` table references.

### 🥇 3. Gold Layer (Business Analytics & Star Schema)
- **`gold_dim_products`**: Product dimension table joined with aisles and departments using `LEFT JOIN` to prevent data loss for unclassified products.
- **`gold_fact_orders`**: Consolidated transactional fact table combining millions of historical order details (`prior` + `train`) via `UNION ALL` and strictly enforced with `INNER JOIN` against primary orders.

---

## 💡 Key Architectural Decisions

* **Star Schema over One Big Table (OBT):** Chosen to maintain storage efficiency, eliminate redundant attributes, and streamline analytical query performance.
* **Referential Integrity Strategy:**
  * **`LEFT JOIN` for Dimensions:** Ensures master catalog completeness even when descriptive metadata is incomplete.
  * **`INNER JOIN` for Facts:** Filters out orphan records and guarantees that every transaction maps to a valid order entity.

---

## 🚀 Potential Future Improvements

- [ ] Add **DLT Data Quality Expectations** (`@dlt.expect_or_drop`, `@dlt.expect`) to monitor null values and invalid foreign keys automatically.
- [ ] Implement **SCD Type 2 (Slowly Changing Dimensions)** using DLT `APPLY CHANGES INTO` for tracking product price/category history over time.
- [ ] Connect the **Gold Layer** to **Power BI / Databricks SQL Dashboards** for real-time market basket insights.
- [ ] Add Automated **CI/CD Workflows** using Databricks Asset Bundles (DABs) or GitHub Actions.

---

## 👤 Author
* **Omar El-Soudy** - [GitHub Profile](https://github.com/omarel-soudy)
 
