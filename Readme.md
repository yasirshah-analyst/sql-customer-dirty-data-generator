# SQL Customer Data Cleaning Project

## 📌 Project Overview

This project demonstrates an end-to-end data cleaning workflow using SQL and Excel.

A deliberately messy customer dataset was first generated in PostgreSQL using SQL scripts containing common real-world data quality issues. The dataset was then exported to Excel, where it was cleaned, validated, and transformed into an analysis-ready dataset.

The project showcases practical data cleaning techniques used by data analysts to improve data quality and ensure accurate reporting and analysis.

---

## 🎯 Project Objectives

- Generate a realistic dirty customer dataset using SQL.
- Simulate common data quality issues found in business datasets.
- Identify and clean inconsistent, incomplete, and invalid records in Excel.
- Standardize text, dates, and numeric fields.
- Handle missing values, duplicates, and formatting issues.
- Produce a clean dataset suitable for analysis and reporting.
- Demonstrate an end-to-end data cleaning workflow for a data analytics portfolio.

---

## 🗂️ Dataset Description

The dataset contains customer information including:

- Customer ID
- Name
- Email
- Phone Number
- City
- Country
- Signup Date
- Amount

A total of 500 records were generated with intentional data quality issues for cleaning practice.

---

## 📁 Project Structure

```text
sql-customer-dirty-data-generator/
│
├── Data/
│   ├── Raw/
│   │   └── customers_dirty.csv
│   │
│   └── clean/
│       └── customers_clean.csv
│
├── SQL/
│   └── customers_dirty_creation.sql
│
├── cleaning/
│   ├── clean_amount_1.png
│   ├── clean_amount_2.png
│   ├── clean_amount_3.png
│   ├── clean_city.png
│   ├── clean_country.png
│   ├── clean_date.png
│   ├── clean_email.png
│   ├── clean_phone_1.png
│   ├── clean_phone_f.png
│   ├── final_clean.png
│   ├── raw_phone.png
│   ├── raw_phone3.png
│   ├── raw_phone_2.png
│   └── trim(b2).png
│
└── README.md
```
---

```

## 🏗️ Create Table

```sql
-- =========================================================
-- CREATE DIRTY CUSTOMER TABLE
-- =========================================================

CREATE TABLE customers_dirty (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT,
    phone TEXT,
    city TEXT,
    country TEXT,
    signup_date TEXT,
    amount TEXT
);
```
---

## 📥 Insert Dirty Data

-- =========================================================
-- INSERT MESSY CUSTOMER DATA
-- =========================================================

```sql
INSERT INTO customers_dirty (
    name,
    email,
    phone,
    city,
    country,
    signup_date,
    amount
)

SELECT

    -- =========================
    -- MESSY NAMES
    -- =========================
    CASE 
        WHEN i % 10 = 0 THEN '  Ali Khan  '
        WHEN i % 7 = 0 THEN 'Sara Ahmed'
        ELSE 'User ' || i
    END,

    -- =========================
    -- INVALID EMAILS
    -- =========================
    CASE 
        WHEN i % 15 = 0 THEN 'invalidemail.com'
        WHEN i % 9 = 0 THEN 'test@test.com'
        ELSE 'user' || i || '@gmail.com'
    END,

    -- =========================
    -- MESSY PHONE NUMBERS
    -- =========================
    CASE 
        WHEN i % 12 = 0 THEN NULL
        WHEN i % 5 = 0 THEN '123-456-789'
        ELSE '03' || (100000000 + i)::TEXT
    END,

    -- =========================
    -- INCONSISTENT CITY NAMES
    -- =========================
    CASE 
        WHEN i % 6 = 0 THEN 'karachi'
        WHEN i % 8 = 0 THEN 'LAHORE'
        WHEN i % 11 = 0 THEN ''
        ELSE 'Islamabad'
    END,

    -- =========================
    -- WRONG COUNTRY VALUES
    -- =========================
    CASE 
        WHEN i % 10 = 0 THEN 'IND'
        ELSE 'Pakistan'
    END,

    -- =========================
    -- MESSY DATE FORMATS
    -- =========================
    CASE 
        WHEN i % 13 = 0 THEN '12-05-2023'
        ELSE '2023-01-' || ((i % 28)+1)
    END,

    -- =========================
    -- OUTLIERS + TEXT VALUES
    -- =========================
    CASE 
        WHEN i % 20 = 0 THEN '999999'
        WHEN i % 17 = 0 THEN 'abc'
        ELSE (100 + i)::TEXT
    END

FROM generate_series(1,500) i;

-- =========================================================
-- 13. FINAL CLEANED DATA PREVIEW
-- =========================================================

select * 
from customers_dirty;
```
---

## Raw Data

[View Raw Dataset](Data/Raw/customers_dirty.csv)

---


## ⚠️ Simulated Data Quality Issues

| Column | Issues Introduced |
|----------|----------|
| Name | Extra spaces, repeated names |
| Email | Invalid and duplicate emails |
| Phone | Missing values and invalid formats |
| City | Inconsistent capitalization and blank values |
| Country | Incorrect country codes |
| Signup Date | Mixed date formats |
| Amount | Non-numeric values and outliers |

---

## 🛠️ Tools Used

- PostgreSQL
- SQL
- Microsoft Excel

---

## 📁 Project Structure

```text
sql-customer-dirty-data-generator/
│
├── SQL/
│   └── customers_dirty_creation.sql
│
├── Data/
│   ├── Raw/
│   │   └── customers_dirty.csv
│   │
│   └── Clean/
│       └── customers_clean.csv
│
├── cleaning/
│   ├── clean_email.png
│   ├── clean_phone.png
│   ├── clean_city.png
│   ├── clean_country.png
│   ├── clean_date.png
│   ├── clean_amount.png
│   └── final_clean.png
│
└── README.md
```

---

# 1️⃣ SQL Data Generation

## Create Table

```sql
CREATE TABLE customers_dirty (
    id SERIAL PRIMARY KEY,
    name TEXT,
    email TEXT,
    phone TEXT,
    city TEXT,
    country TEXT,
    signup_date TEXT,
    amount TEXT
);
```

## Generate Dirty Data

The dataset was intentionally populated with:

- Invalid email addresses
- Duplicate email records
- Missing phone numbers
- Incorrect phone formats
- Inconsistent city capitalization
- Incorrect country values
- Mixed date formats
- Text values in numeric fields
- Numeric outliers

using SQL `CASE WHEN` statements and `generate_series()`.

---

## 📥 Raw Dataset

[View Raw Dataset](Data/Raw/customers_dirty.csv)

---

# 2️⃣ Excel Data Cleaning Process

## Step 1: Inspect the Dataset

### Checked all columns for:

- Missing values
- Duplicate records
- Invalid formats
- Inconsistent text
- Outliers
- Incorrect data types

---

## Step 2: Clean Name Column

### Issues Found

- Leading and trailing spaces
- Repeated names

### Actions Taken

- Removed extra spaces using `TRIM()`
- Standardized name formatting
- Verified duplicates using ID and Email

### Formula Used

```excel
=TRIM(B2)
```

---

## Step 3: Clean Email Column

### Issues Found

- Invalid email addresses
- Duplicate email records
- Test/Fake email addresses

### Examples

- invalidemail.com
- test@test.com

### Actions Taken

- Flagged invalid email formats
- Identified test/fake emails
- Created Email Status classification
- Excluded invalid records from analysis

### Email Status Categories

- Valid
- Invalid Format
- Test/Fake Email

### Cleaning Evidence

![Email Cleaning](cleaning/clean_email.png)

---

## Step 4: Clean Phone Number Column

### Issues Found

- Invalid phone format
- Missing values

### Examples

- 123-456-789
- NULL

### Actions Taken

- Removed invalid formatting
- Converted NULL values to blanks
- Validated phone number length
- Flagged invalid phone records

### Cleaning Evidence

![Phone Cleaning](cleaning/clean_phone.png)

---

## Step 5: Clean City Column

### Issues Found

- Islamabad
- karachi
- LAHORE

### Actions Taken

- Standardized city names
- Applied proper capitalization

### Formula Used

```excel
=PROPER(E2)
```

### Standardized Values

- Islamabad
- Karachi
- Lahore

### Cleaning Evidence

![City Cleaning](cleaning/clean_city.png)

---

## Step 6: Clean Country Column

### Issues Found

- Pakistan
- IND

### Actions Taken

- Standardized country names
- Replaced abbreviations with full country names

### Example

| Before | After |
|----------|----------|
| IND | India |

### Cleaning Evidence

![Country Cleaning](cleaning/clean_country.png)

---

## Step 7: Handle Missing City Values

### Issues Found

Blank city records.

### Actions Taken

- Identified missing values
- Replaced with "Unknown" where appropriate

---

## Step 8: Clean Amount Column

### Issues Found

Non-numeric values:

- abc

Outliers:

- 999999

### Actions Taken

- Converted values to numeric format
- Flagged non-numeric entries
- Investigated extreme outliers

### Cleaning Evidence

![Amount Cleaning](cleaning/clean_amount.png)

---

## Step 9: Validate Date Column

### Issues Found

Mixed date formats.

### Examples

- 1/2/2023
- 12/5/2023

### Actions Taken

- Converted all dates to a consistent format
- Standardized dates using ISO format

### Final Format

```text
YYYY-MM-DD
```

### Cleaning Evidence

![Date Cleaning](cleaning/clean_date.png)

---

## Step 10: Verify ID Column

### Actions Taken

- Checked for missing IDs
- Checked for duplicate IDs
- Confirmed primary key integrity

---

# 3️⃣ Cleaned Dataset

After completing the cleaning process, a clean version of the dataset was created for analysis and reporting.

[View Clean Dataset](Data/Clean/customers_clean.csv)

### Final Output

![Final Clean Dataset](cleaning/final_clean.png)

---

# ✅ Key Cleaning Outcomes

- Removed leading and trailing spaces from text fields.
- Identified invalid and duplicate email records.
- Standardized phone number formats and handled missing values.
- Corrected inconsistent city and country values.
- Converted dates into a consistent format.
- Validated numeric fields and flagged outliers.
- Verified dataset integrity through duplicate and missing-value checks.
- Produced a clean, analysis-ready customer dataset.

---

## 🧠 SQL Concepts Used

- CREATE TABLE
- INSERT INTO
- CASE WHEN
- generate_series()

---

## 👤 Author

**Yasir Shah**

Aspiring Data Analyst | SQL | Excel | Data Cleaning | Data Analytics
