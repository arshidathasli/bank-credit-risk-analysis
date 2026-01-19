Bank Transaction Risk Analysis

(Python · MySQL · Data Analysis)

Project Overview

This project performs an end-to-end analysis of credit card transaction data to identify fraud patterns, customer behavior, and transaction anomalies using Python and MySQL.

The objective is to simulate a real-world banking risk analytics workflow, focusing on data quality, SQL-based analysis, and interpretable insights rather than purely model-driven approaches.

Business Problem

Banks handle millions of transactions daily, where fraudulent activity is rare but financially significant. Detecting such activity requires careful analysis of transaction behavior, customer spending patterns, and deviations from normal trends.

This project addresses the following questions:

How do fraudulent transactions differ from normal transactions?

Can sudden transaction spikes indicate potential fraud?

Which customer or transaction characteristics show higher risk?

How can SQL window functions support anomaly detection?

Dataset

Source: Public credit card transaction dataset (synthetic banking data)

Size: Approximately 1.29 million transactions

Key fields:

transaction_time – Timestamp of the transaction

card_number – Customer identifier

merchant, category – Merchant details

amount – Transaction amount

city, state – Location details

is_fraud – Fraud label (0 = non-fraud, 1 = fraud)

Fraud transactions represent less than 1% of the dataset, reflecting real-world banking imbalance.

Tools and Technologies

Python (Pandas, NumPy)

MySQL

SQL (aggregations, CTEs, window functions)

Matplotlib, Seaborn

Jupyter Notebook

Project Structure
bank-credit-risk-analysis/
│
├── data/
│   ├── raw/
│   └── cleaned/
│
├── notebooks/
│   ├── 01_data_inspection.ipynb
│   ├── 02_data_cleaning.ipynb
│   ├── 03_load_to_mysql.ipynb
│   └── 04_visualization.ipynb
│
├── sql/
│   └── analysis_queries.sql
│
├── report/
│   └── insights.md
│
└── README.md

Data Cleaning and Preparation

The following steps were performed to ensure data quality:

Removed unnecessary index columns

Converted transaction timestamps to datetime format

Standardized column names for SQL compatibility

Removed invalid records such as negative transaction amounts

Checked for duplicates and missing values

Saved cleaned data for database loading

Database Design

A transactions table was created in MySQL with appropriate data types for financial data.
The schema supports large-scale analytical queries and preserves transactional granularity.

Key considerations:

Proper numeric precision for monetary values

Primary key on transaction identifier

Efficient querying for time-based and customer-level analysis

SQL Analysis

Key analyses performed using SQL include:

Fraud vs non-fraud distribution

High-spending and high-activity customers

Fraud rate by geographic region

Time-based fraud analysis (hour of day)

Customer-level anomaly detection using rolling averages

Rolling average logic example:

AVG(amount) OVER (
    PARTITION BY card_number
    ORDER BY transaction_time
    ROWS BETWEEN 5 PRECEDING AND CURRENT ROW
)


This approach enables comparison of each transaction against recent customer behavior without aggregating away transaction-level detail.

Visual Analysis

Two focused visualizations were used to support findings:

Comparison of transaction amount distributions for fraud and non-fraud cases

Time-series analysis of transaction behavior for individual cards

The emphasis was on clarity and interpretability rather than volume of charts.

Key Insights

Fraud transactions show significantly higher variance than normal transactions

Fraud data is highly imbalanced, consistent with real banking environments

Sudden transaction spikes often indicate abnormal behavior

Rule-based anomaly detection using rolling averages is effective as an initial risk filter

Key Learnings

Importance of data validation in financial analytics

Practical application of SQL window functions

Integration of Python and MySQL for large datasets

Business-driven interpretation of analytical results

Future Enhancements

Introduce machine learning-based fraud detection models

Add dashboards using Power BI or Tableau

Automate data ingestion and validation pipelines

Optimize anomaly thresholds to reduce false positives