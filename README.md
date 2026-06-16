# International Student Admissions & Outreach Dashboard

## 📌 Project Overview

This project provides a comprehensive data analytics and visualization solution for international student admissions and outreach. It transforms raw, messy application data into a clean, structured, and interactive dashboard, enabling stakeholders to monitor key performance indicators (KPIs), track student progression, and make data-driven decisions.

The pipeline includes:
- **Data Cleaning & EDA:** Handling missing values, outliers, and inconsistencies in applicant, SEVIS, outreach, and deposit datasets.
- **Database Engineering:** Creating a relational database (PostgreSQL) to store and link data from multiple sources.
- **Dashboard Development:** Building an interactive Looker Studio dashboard with filters for dynamic exploration of admissions, engagement, and immigration metrics.
- **Insight Extraction & Validation:** Generating actionable insights from the data and validating key dashboard metrics using backend SQL queries.

## 🚀 Key Features

- **📊 Interactive Dashboard:** A user-friendly Looker Studio dashboard that provides a high-level snapshot of key metrics like total applicants, top programs, country distribution, and outreach status.
- **🔍 Data Quality & EDA:** Detailed analysis of data completeness, identification of high-cardinality and constant columns, and detection of outliers to ensure data reliability.
- **📈 Conversion Funnel Analysis:** Visualizes the progression of applicants from initial outreach to visa approval, identifying potential bottlenecks.
- **🌍 Demographic & Geographic Insights:** Highlights applicant concentration by country, program, and application source to inform recruitment strategies.
- **🔗 Validated Metrics:** All dashboard KPIs have been cross-validated against backend SQL queries to ensure consistency and accuracy.

## 🛠️ Technologies Used

- **Data Analysis:** Python (Pandas, NumPy)
- **Database:** PostgreSQL (Supabase)
- **Visualization:** Looker Studio
- **Version Control:** Git & GitHub

## 📁 Project Structure
.
├── data/ # Raw and cleaned CSV files (if not sensitive)
│ ├── applicant.csv
│ ├── connect.csv
│ ├── outreach.csv
│ └── sevis.csv
├── notebooks/ # Jupyter notebooks for EDA and cleaning
│ ├── Week-1_EDA_and_Cleaning.ipynb
│ └── Database_Preprocessing.ipynb
├── scripts/ # Python scripts for data cleaning and upload
│ └── clean_and_upload.py
├── sql/ # SQL scripts for schema definition and validation
│ ├── schema.sql
│ └── validation_queries.sql
├── docs/ # Detailed technical documentation
│ └── TECHNICAL_README.md
├── reports/ # Weekly progress reports (PDFs)
│ ├── Week-1_Amna Zubair_Team-16.pdf
│ ├── Week-2_Amna Zubair_Team-16.pdf
│ └── Week-3_Amna Zubair_Team-16.pdf
└── README.md # This file


## 🎯 Key Insights

- **Applicant Concentration:** The applicant pool is heavily concentrated in a few countries (India, Nigeria, Ghana) and a select group of graduate programs (Analytics, Information Systems, MBA), indicating strong demand in specific markets.
- **Recruitment Channels:** Platforms like `INTO` and `Slate` are the dominant application sources, suggesting these channels are highly effective.
- **Pipeline Bottleneck:** A significant portion of students are in the early to mid-stages of the outreach pipeline (`Outreach efforts`, `Appointment scheduled`). This presents a clear opportunity to optimize conversion strategies from outreach to visa approval.
- **Data Quality:** Initial datasets contained a high percentage of missing values and anomalies. A rigorous cleaning and imputation process was essential to prepare the data for analysis.

## 🗄️ Dashboard

A functional Looker Studio dashboard was developed based on the cleaned data. It can be viewed here:

🔗 **[Link to Looker Studio Dashboard](https://lookerstudio.google.com/u/0/reporting/74096464-96ff-45bc-8ecc-ab8535a3d44c/page/avllF/edit)**

*Please note: The dashboard may not be publicly accessible if it is set to private.*

## 👥 Team

- **Amna Zubair** - Data Analytics & Visualization
- **Sathwika Rupireddy** - Data Analytics & Visualization

## 📅 Project Timeline

- **Week 1:** Data cleaning, EDA, and missing value imputation.
- **Week 2:** Database setup, dashboard development, and initial insights.
- **Week 3:** Insight validation and reporting.
