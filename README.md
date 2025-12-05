# OnlineRetail_data_analysis Project Documentation
## Phase 1 — Data Ingestion Summary

### **Overview**
This phase focuses on loading and performing initial exploratory analysis on the "Online Retail" dataset stored in an Excel file.

### **Steps Performed**

#### 1. **Package Installation & Imports**
- Installed required packages: `sqlalchemy`, `psycopg2-binary`, `pandas`, `matplotlib`, `seaborn`, `numpy`, `python-dotenv`.
- Imported `pandas` for data manipulation.

#### 2. **Data Loading**
- Loaded the dataset from `'online_retail.xlsx'` into a DataFrame named `online_retail`.

#### 3. **Initial Data Exploration**
- **`.head()`**: Displayed the first few rows to review raw data structure.
- **`.shape()`**: Checked the dataset's dimensions (rows and columns).
- **`.describe()`**: Generated descriptive statistics (e.g., mean, std, min/max) for numerical columns.
- **`.info()`**: Provided a summary of the DataFrame, including column names, data types, and non-null counts.

### **Purpose**
To ingest and perform a preliminary review of the dataset, identifying its structure, size, data types, and basic statistical properties before further analysis.

---

## Phase 2: Data Storage (SQL Layer) Summary

This Python script handles the data storage phase, loading raw retail data into a SQL database:

### Purpose
- Load raw transaction data from an Excel file into a PostgreSQL database
- Serve as the data persistence layer in the data pipeline

### Key Components

#### 1. Imports & Setup
```
import pandas as pd
from sqlalchemy import create_engine
from dotenv import load_dotenv
import os
```

#### Libraries Used
- **Pandas**: For reading Excel data
- **SQLAlchemy**: Database ORM and connection management
- **python-dotenv**: Secure credential management
- **os**: Environment variable access

#### 2. Data Loading
- Reads data from `online_retail.xlsx` into a pandas DataFrame
- Uses environment variables for secure database connection configuration

#### 3. Database Operations
- Creates a database engine using connection URL from environment variables
- Writes the DataFrame to PostgreSQL with:
  - Table name: `online_retail_data`
  - Schema: `online_retail`
  - Behavior: Replace existing table (`if_exists='replace'`)
  - No index column: `index=False`

#### Workflow
1. Load environment variables with database credentials
2. Read Excel data into pandas DataFrame
3. Establish database connection
4. Write DataFrame to PostgreSQL table in specified schema
5. Table is replaced if it already exists

#### Security Features
- Credentials stored in `.env` file (not hardcoded)
- Connection string managed via environment variables

---

## Phase 3: Data Preparation (Cleaning & Transformation) Summary

### Overview
This Python script performs data cleaning and transformation on the `online_retail.xlsx` dataset using pandas and SQLAlchemy, then stores the cleaned data in a database.

### Key Operations
#### 1. Data Loading
- Import `online_retail.xlsx` into a pandas DataFrame

#### 2. Data Cleaning Steps
- **Quantity Filtering**: Remove transactions with negative or zero quantities
- **Price Filtering**: Remove rows with negative UnitPrice
- **Type Conversion**: Convert 'InvoiceNo' column to string type
- **Cancellation Removal**: Exclude invoice cancellations (InvoiceNo starting with "C")
- **Missing Data**: Drop rows with missing CustomerID
- **Duplicate Removal**: Eliminate duplicate records

#### 3. Feature Engineering
- **Revenue Calculation**: Create new Revenue column (Quantity × UnitPrice)
- **Date Components Extraction**: Create Month, Year, and Day columns from InvoiceDate

#### 4. Data Storage
- Load database URL from environment variables
- Store cleaned data as `clean_online_retail_data` table in PostgreSQL database
- Uses `if_exists='replace'` to overwrite existing table
- Stores in `online_retail` schema

### Libraries Used
- **pandas** - Data manipulation
- **sqlalchemy** - Database connection
- **dotenv & os** - Environment variable management

### Output
- Cleaned DataFrame (`dropped_duplicates`)
- Persisted cleaned data to PostgreSQL database table

---

## Phase 4: Data Analysis Summary

### Overview
This Python script performs sales data analysis using a prepared dataset imported from a `data_preparation` module.

### Analysis Sections
#### 1. 2011 Monthly Revenue Analysis
- Filters data for year 2011 only
- Groups by month and calculates total revenue
- Sorts months by revenue in descending order
- **Purpose**: Identify seasonal patterns and month-to-month changes

#### 2. Country Performance Analysis (Excluding UK)
- Excludes United Kingdom from analysis
- Ranks countries by:
  - Total revenue (top 10 highlighted)
  - Total quantity sold (top 10 highlighted)
- Provides insights into international market performance

#### 3. Customer Analysis
- Ranks all customers by total revenue spent
- Identifies top 10 highest-spending customers
- Helps identify valuable customer segments

#### 4. Global Product Demand Analysis
- Computes total quantity sold per country
- Excludes United Kingdom
- Identifies top 10 "high-opportunity markets" based on quantity demanded
- **Purpose**: Highlight markets with highest demand for expansion opportunities

### Key Operations
- **Grouping**: Extensive use of `groupby()` operations
- **Aggregation**: `sum()` for revenue and quantity calculations
- **Filtering**: Multiple filters applied for year and country exclusions
- **Sorting**: Results consistently sorted in descending order
- **Top-N Analysis**: Frequent use of `.head(10)` to highlight top performers

### Data Structure
The analysis assumes the dataset contains these columns:
- `Year`, `Month`, `Revenue`, `Quantity`, `Country`, `CustomerID`

### Business Insights Generated
- Monthly revenue trends for 2011
- Country rankings for revenue and quantity (international focus)
- Customer value segmentation
- Market opportunity identification based on demand
