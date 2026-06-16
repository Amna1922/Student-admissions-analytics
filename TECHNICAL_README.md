# Technical Documentation: International Student Admissions & Outreach Dashboard

## 1. Introduction

This document provides a detailed technical overview of the data engineering, cleaning, and validation pipeline used to build the International Student Admissions & Outreach Dashboard. It is intended for data engineers, analysts, and other technical stakeholders who need to understand the architecture and data flow.

## 2. Data Sources & Schema

The project ingests data from four primary CSV files, each representing a different facet of the student lifecycle.

### 2.1. Applicant Dataset
- **File:** `applicant.csv`
- **Description:** Contains core student application information (demographics, program of interest, intake term).
- **Key Columns:** `Reference_ID`, `Given_Name`, `Last_Name`, `Major`, `Citizenship`, `Intake`, `Email_ID`.
- **Pre-Cleaning Size:** 6,895 rows, 30 columns.

### 2.2. Outreach Dataset
- **File:** `outreach.csv`
- **Description:** Tracks communication and engagement activities with prospective students.
- **Key Columns:** `Reference_ID`, `Student_Status`, `Application_Source`, `Program_Of_interest`.
- **Size:** 1,565 rows, 18 columns.

### 2.3. SEVIS Dataset
- **File:** `sevis.csv`
- **Description:** Contains immigration and compliance information for international students.
- **Key Columns:** `SEVIS_ID`, `Reference_ID`, `SEVIS_Status`, `Visa_Type`, `Country_of_Citizenship`.
- **Size:** 3,852 rows, 123 columns (initial).

### 2.4. Connect/Deposit Dataset
- **File:** `deposit.csv`
- **Description:** Captures enrollment deposit and I-20 status information.
- **Key Columns:** `Reference_ID`, `Deposit_Status`, `I_20_Status`, `Assigned` (staff).
- **Size:** 34,341 rows, 8 columns.

### 2.5. Database Schema (PostgreSQL)
A relational database was designed in Supabase (PostgreSQL) to store the cleaned data.

- **Tables:** `sevis`, `applicant`, `deposit`, `outreach`.
- **Primary Keys:** Surrogate keys were used where business keys were not unique.
- **Foreign Keys:** Relationships were established using `Reference_ID` and `SEVIS_ID` to link records across tables.
  - `applicant.reference_id` → `sevis.reference_id`
  - `deposit.reference_id` → `applicant.reference_id`
  - `outreach.reference_id` → `applicant.reference_id`

## 3. Data Cleaning & Preprocessing

A significant portion of the work involved cleaning the raw data to ensure accuracy.

**Key Cleaning Steps:**

1.  **High-Missingness Column Removal:** Columns with >70% missing values were dropped (e.g., `Admit_Date`, `Comments`, `Thesis_Or_Dissertation`, `SEVIS Legacy Last Name`).
2.  **Constant Value Columns:** 25 columns in the SEVIS dataset containing the same value for all rows were dropped (e.g., `School Code`, `CPT Approved`, `STEM OPT Requested`).
3.  **Handling Duplicates:** Full-row duplicates were absent. However, a few duplicates were found in identifier columns (`Reference_ID`, `Email_ID`) and were removed. Name-based duplicate checks were also performed.
4.  **Missing Value Imputation:**
    - **Categorical:** Filled with the mode (e.g., `Region` → 'Telangana', `Citizenship` → 'India').
    - **Identifiers:** Filled with `Unknown` or `Not_Specified` (e.g., `Postal`, `Application_Agency_Code`).
    - **SEVIS_ID:** Created a new column (`SEVIS_ID_missing`) as a flag and filled missing values with `Not_Available`.
5.  **Outlier Handling:**
    - **Applicant Dataset:** Outliers in `Reference_ID` were capped using the IQR method.
    - **Outreach Dataset:** Outliers were detected in text length fields (e.g., `Name_Length`, `Email_Length`, `Contact_Length`). Most were found in `Contact_Length` (18%), primarily due to country codes or missing data.
6.  **Date/Time Standardization:** Converted various date formats (e.g., DD/MM/YYYY) into PostgreSQL-compliant `YYYY-MM-DD` format.
7.  **Contact Number Cleanup:** Identified that 82% of contact numbers were valid 10-digit numbers, while others were identified as anomalous and flagged for review.

## 4. Data Upload & Database Challenges

Uploading the data to the PostgreSQL database presented several challenges.

| Challenge | Solution |
| :--- | :--- |
| **Foreign Key Constraint Violations** | Adopted a "Upload First, Match Later" strategy. Foreign key columns were set to `NULL` during upload. After all data was loaded, SQL `UPDATE` queries with `JOINs` were used to link records based on fields like email, name, and date of birth. |
| **Date Format Incompatibility** | Pre-processing Python scripts were created to automatically detect and convert date formats (DD/MM/YYYY to YYYY-MM-DD). |
| **Duplicate Primary Key Violations** | For the `deposit` table, a surrogate `id` (SERIAL) key was used to allow duplicates in the business key temporarily. |
| **Non-Unique Foreign Key References** | Used existing unique columns like `reference_id` where possible and added a `applicant_reference_id` column to the `outreach` table for proper linking. |
| **Empty String vs. NULL Handling** | Modified Python scripts to convert empty strings to actual `NULL` values and altered column definitions to allow `NULL` where appropriate. |

## 5. Dashboard & Insight Validation

The final dashboard metrics were validated by comparing them against SQL aggregations performed on the cleaned database.

**Key Validations Performed:**

1.  **Total Applicants:** Dashboard KPI (1,565) confirmed by `COUNT(*)` on the applicant table.
2.  **Application Source:** Dashboard bar chart ranking (`INTO` and `Slate` as top sources) was validated by `GROUP BY application_source` SQL queries.
3.  **Student Status:** Dashboard proportions for statuses like "Outreach efforts" and "Visa Approved" were validated by SQL grouping on `student_status`.
4.  **Program Pipeline:** Dashboard visuals showing conversion patterns by program were confirmed via SQL, which also showed variation in acceptance, deposit, and visa approval.

**No major discrepancies were found between the dashboard and the validated SQL queries.**

## 6. Reproducibility

To reproduce the data pipeline:

1.  **Set up the database:** Run the `sql/schema.sql` script in a PostgreSQL environment (e.g., Supabase).
2.  **Preprocess the data:** Use the Python scripts in the `scripts/` directory to clean the raw CSV files.
3.  **Upload the data:** Use the Supabase web interface or the `clean_and_upload.py` script to load the cleaned data into the tables.
4.  **Link the records:** Execute the matching SQL queries to link the data via foreign keys (if the "Upload First, Match Later" strategy is used).
5.  **Connect to Looker Studio:** Use the PostgreSQL connector in Looker Studio to connect to the database and build or view the dashboard.
