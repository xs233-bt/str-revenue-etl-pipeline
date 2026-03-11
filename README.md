# STR Revenue Automated ETL Pipeline

## 📌 Project Overview

This repository contains a **production-style automated ETL pipeline** designed to extract, transform, and deliver **STR revenue data** from a secure AWS environment into **analytics-ready datasets** for business intelligence and stakeholder consumption.

The project demonstrates **end-to-end data engineering ownership**, combining:
- Secure data access
- Automated orchestration
- Scalable data transformation
- BI-optimized outputs
- Stakeholder-friendly tooling

---

## 🧭 High-Level Architecture

**Daily automated data flow:**

- **Airflow DAG**
- ↓ Python SSH Tunnel (EC2 Bastion)
- ↓ AWS RDS MySQL (Source)
- ↓ Python Transform (Pandas / PyArrow)
- ↓ Parquet Storage (OneDrive)
- ↓ Power BI Dashboards


---

## 🏗️ Architecture Breakdown

### 1️⃣ Airflow DAG — Orchestration Layer

- Runs on a **daily schedule**
- Manages task dependencies, retries, and failures
- Acts as the control plane for the entire pipeline

**Responsibilities**
- Trigger secure database connectivity
- Execute extraction and transformation logic
- Ensure pipeline reliability and repeatability

---

### 2️⃣ Secure SSH Tunnel — Network & Security Layer

- Uses an **EC2 Jump Host (Bastion)**
- Establishes an SSH tunnel programmatically using Python:
  - `sshtunnel`
  - `paramiko`
- Forwards:
  - Local port → `AWS RDS MySQL (3306)`

**Why this design**
- RDS is not publicly exposed
- No inbound database access from the internet
- Aligns with enterprise security best practices

---

### 3️⃣ AWS RDS MySQL — Source System

- Acts as the **system of record** for STR revenue
- Contains transactional operational data
- Accessed exclusively through the secured SSH tunnel

---

### 4️⃣ Python Transform Layer — ELT Core

- Extracts data via the forwarded local port
- Transforms data using:
  - **Pandas** for data manipulation
  - **PyArrow** for Parquet serialization

**Transform responsibilities**
- Data cleaning and normalization
- Metric standardization
- Schema stabilization for BI tools

---

### 5️⃣ Parquet Storage — Analytics Layer

Data is written in **Parquet format** and organized into two logical zones:

#### 📂 Historical
- Immutable snapshots of all STR data
- Supports:
  - Backfills
  - Audits
  - Reprocessing

#### 📂 Current
- Latest daily dataset only
- Optimized for fast BI refresh and reporting

**Why Parquet**
- Columnar and compressed
- Storage- and query-efficient
- Native compatibility with modern BI tools

---

### 6️⃣ BI Consumption — Power BI

- Power BI reads Parquet outputs directly
- Enables:
  - Daily revenue dashboards
  - Trend analysis
  - Operational monitoring

---

## 🧰 Stakeholder Tool: Parquet → CSV Converter (GUI)

### 🎯 Purpose

While Parquet is optimal for analytics systems, **business stakeholders** often require **CSV files** for:

- Excel review
- Manual validation
- Ad-hoc sharing

This repository includes a **desktop GUI application** that allows **non-technical users** to convert Parquet files into CSV without using the command line.

---

### 🖥️ Tool Features

- Select **one or multiple Parquet files**
- Convert each file into CSV
- Choose output directory interactively
- Optional ZIP packaging of all CSV outputs
- Real-time status logging in the UI
- No Python or technical knowledge required

---

### 🧠 Tool Architecture (High Level)

- **Tkinter** — Desktop GUI
- **Pandas** — Parquet ingestion and CSV output
- **PyArrow / Fastparquet** — Parquet engine
- **Zipfile** — Optional output compression

---

### 🧩 Tool Workflow

1. User selects Parquet files via file picker
2. Application reads each file using Pandas
3. Files are written as CSV
4. (Optional) All CSVs are zipped for easy sharing
5. Status messages are displayed in the UI

---

## 📁 Repository Structure

- str-revenue-etl-pipeline/
  - dags/ — Airflow DAG definitions
  - src/ — ETL & transformation logic
  - tools/
    - parquet_to_csv/ — Stakeholder GUI utility
  - docs/ — Architecture diagrams
  - README.md



---

## 🛠️ Tech Stack

- Apache Airflow — Orchestration
- Python — ETL & tooling
- Pandas / PyArrow — Data transformation
- AWS EC2 — SSH Bastion host
- AWS RDS MySQL — Source database
- OneDrive — Analytics storage layer
- Power BI — Visualization & reporting
- Tkinter — Stakeholder GUI tool

---

## 🧠 Key Engineering Decisions

| Decision | Rationale |
|--------|----------|
| SSH Bastion Host | Secure database access without public exposure |
| Airflow Orchestration | Production-grade scheduling and retries |
| Parquet Output | BI-optimized and storage-efficient |
| Historical + Current Zones | Auditability and fast dashboard refresh |
| GUI Tool for CSV | Reduces ad-hoc data requests from stakeholders |

---

## 📈 Portfolio Value

This project demonstrates:

- Real-world enterprise security patterns
- End-to-end ETL pipeline ownership
- ELT-oriented architecture design
- Analytics-focused data modeling
- Stakeholder-centric tooling

---

## 🔮 Future Enhancements

- Data quality checks (Great Expectations)
- Schema evolution handling
- Iceberg / Delta Lake integration
- Data lineage & metadata tracking
- Automated anomaly detection and alerts

---

## 🧾Summary

> Designed and implemented a secure, Airflow-orchestrated ETL pipeline to extract STR revenue data from AWS RDS via SSH bastion, transform it using Python, and deliver Parquet-based datasets for Power BI, including a GUI tool enabling non-technical stakeholders to self-serve CSV exports.


