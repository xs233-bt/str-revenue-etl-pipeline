📌 Overview

This project implements a production-style automated ETL pipeline that extracts STR revenue data from a secure AWS RDS MySQL database, transforms it using Python, and delivers analytics-ready Parquet datasets for downstream BI consumption.

The pipeline is designed to mirror real enterprise data engineering workflows, with strong emphasis on:

Secure network access

Automated orchestration

Historical data preservation

BI-optimized outputs

🧭 High-Level Architecture

Daily automated flow:

Airflow DAG
   ↓
Python SSH Tunnel (Bastion)
   ↓
AWS RDS MySQL
   ↓
Python Transform (Pandas / PyArrow)
   ↓
Parquet Storage (OneDrive)
   ↓
Power BI Dashboards

🏗️ Architecture Breakdown
1️⃣ Airflow DAG — Orchestration Layer

Runs on a daily schedule

Controls task sequencing, retries, and failure handling

Serves as the single entry point for the entire pipeline

Key responsibilities

Establish secure database access

Trigger extraction & transformation tasks

Ensure pipeline reliability and observability

2️⃣ Secure SSH Tunnel — Network & Security Layer

Uses an EC2 Jump Host (Bastion)

Establishes an SSH tunnel programmatically via Python:

sshtunnel

paramiko

Forwards:

Local port → AWS RDS MySQL : 3306

Why this design

RDS is not publicly accessible

No inbound database exposure

Aligns with enterprise security best practices

3️⃣ AWS RDS MySQL — Source System

Acts as the system of record for STR revenue

Contains transactional operational data

Accessed only through the SSH tunnel

4️⃣ Python Transform Layer — ELT Core

Extracts data via the forwarded local port

Transforms data using:

Pandas for data manipulation

PyArrow for Parquet serialization

Transform responsibilities

Data cleaning & type normalization

Metric standardization

Schema stabilization for BI tools

5️⃣ Parquet Storage — Analytics Layer

Data is written in Parquet format and organized into two logical zones:

📂 Historical

Immutable snapshots of all STR data

Supports:

Backfills

Audits

Reprocessing

📂 Current

Latest daily dataset only

Optimized for:

Fast BI refresh

Low query overhead

Why Parquet

Columnar format

Efficient storage

Native compatibility with Power BI & modern analytics tools

6️⃣ BI Consumption — Power BI

Power BI reads Parquet outputs directly

Enables:

Daily revenue dashboards

Trend analysis

Operational monitoring

📁 Repository Structure
str-revenue-etl-pipeline/
├── dags/                     # Airflow DAG definitions
├── src/                      # ETL & transformation logic
├── tools/
│   └── parquet_to_csv/       # Stakeholder utility tool
├── docs/
│   └── str_revenue_etl_architecture.png
├── README.md

🛠️ Tech Stack

Apache Airflow — Orchestration

Python — Core ETL logic

Pandas / PyArrow — Transformation & Parquet output

AWS EC2 — SSH Bastion host

AWS RDS MySQL — Source database

OneDrive — Analytics storage layer

Power BI — Visualization & reporting

🧠 Key Engineering Decisions
Decision	Rationale
SSH Bastion	Secure access without public DB exposure
Parquet Output	BI-optimized, cost-efficient storage
Historical + Current split	Supports audit & fast dashboards
Airflow Orchestration	Production-grade scheduling & retries
Python-based ELT	Flexibility and testability
📈 Why This Project Matters (Portfolio Perspective)

This project demonstrates:

Real-world enterprise security patterns

End-to-end production ETL ownership

Strong understanding of ELT vs ETL

Analytics-driven data modeling

Stakeholder-friendly data delivery

🧪 Future Enhancements

Data quality checks (Great Expectations)

Schema evolution handling

Iceberg / Delta Lake integration

Metadata & data lineage tracking

Automated alerting on data anomalies
