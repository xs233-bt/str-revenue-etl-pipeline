📦 Parquet → CSV Converter (Stakeholder-Friendly Data Utility)
🚀 Project Overview

This project is a desktop GUI application built in Python that enables non-technical stakeholders to convert Parquet files into human-readable CSV format without using command-line tools or writing code.

It addresses a common real-world data problem:

Modern data pipelines store data efficiently in Parquet,
but business users still need CSV for manual review, validation, and ad-hoc analysis.

This tool bridges that gap.

💡 Business Problem

In analytics and data engineering environments:

Data is stored in columnar formats (Parquet) for performance and cost

Stakeholders often rely on Excel / CSV

Asking business users to install Python or run scripts creates friction

This application removes that friction by providing a simple, self-service interface.

🎯 Key Features

✅ Select multiple Parquet files

🔄 Convert each file into CSV

📂 Choose output directory interactively

🗜️ Optional ZIP packaging for easy sharing

📜 Real-time status logging in the UI

🖱️ No command line or technical knowledge required

🖥️ Application Workflow

User selects one or more .parquet files

App loads each file using Pandas

Files are converted to .csv

(Optional) CSVs are compressed into a ZIP archive

Conversion results are displayed in the UI

🧠 Architecture & Design

Why this design?

GUI over CLI → lowers adoption barrier for business users

Batch processing → efficient handling of multiple files

Stateless execution → safe for ad-hoc usage

Core Components
Component	Purpose
tkinter	Desktop user interface
pandas	Parquet → CSV transformation
zipfile	Optional CSV compression
Event-driven callbacks	User-triggered execution
🧩 Code Structure (High-Level)
parquet-to-csv-gui/
├── app.py
├── requirements.txt
└── README.md

Key Functions

select_files() – Handles file selection via GUI

convert_and_zip() – Converts Parquet files and optionally zips output

start_conversion() – Validates inputs and orchestrates execution

log() – Displays execution feedback to the user

🛠️ Tech Stack

Python 3

Pandas

PyArrow / Fastparquet

Tkinter

Zipfile

📦 Installation
pip install pandas pyarrow


tkinter is included with most Python installations.

👤 Target Users

Business stakeholders

Finance & Operations teams

Analysts without Python experience

Data engineers supporting self-service analytics

🧪 Engineering Considerations

Handles multiple files in a single run

Prevents conversion if no files are selected

Logs both success and failure cases

Designed to be extensible for validation or schema checks
