# Walmart Sales Data Analysis : End-to-End SQL + Python Project

## Project Overview
This project is an end-to-end data analysis solution aimed at extracting valuable business insights from Walmart sales data. Using Python for data processing and analysis, SQL for advanced querying, and structured problem-solving techniques, I explore key sales trends and performance metrics.

## Requirements
- **Python 3.8+**
- **SQL Databases**: MySQL
- **Python Libraries**:
  - `pandas`, `pymysql`, `sqlalchemy`, `mysql-connector-python`
- **Kaggle API Key** (for data downloading)

## Project Workflow

### 1. Environment Setup
- Configured a structured workspace within VS Code for efficient data handling and analysis.
- Installed necessary dependencies using:
  ```bash
  pip install pandas pymysql sqlalchemy mysql-connector-python
  ```

### 2. Kaggle API Setup & Data Acquisition
- Obtained Kaggle API credentials and configured them locally.
- Used the Kaggle API to download the **Walmart Sales Dataset**: [Dataset Link](https://www.kaggle.com/najir0123/walmart-10k-sales-datasets)
- Stored the dataset in the `data/` folder for streamlined access.

### 3. Data Exploration & Cleaning
- Loaded the dataset into Pandas and performed an initial assessment using:
  ```python
  df.shape
  df.head()
  df.describe()
  df.info()
  ```
- Checked for data quality issues:
  ```python
  # No. of duplicates
  df.duplicated().sum()

  # No. of missing values
  df.isnull().sum()
  ```
- Data Cleaning Steps:
  ```python
  # Dropping duplicates
  df.drop_duplicates(inplace=True)
  df.duplicated().sum()

  # Dropping missing values
  df.dropna(inplace=True)
  df.isnull().sum()
  ```
- Standardized pricing data:
  ```python
  # Removing $ sign and converting to float
  df['unit_price'] = df['unit_price'].str.replace('$','').astype(float)
  ```

### 4. Feature Engineering
- Created a **Total Sales Amount** column for revenue calculations.
- Enhanced dataset usability for SQL queries and aggregations.

### 5. Database Integration & SQL Execution
- Connected to MySQL using SQLAlchemy and PyMySQL.
- Created tables and inserted cleaned data.
- Verified successful data ingestion through exploratory SQL queries.

### 6. SQL Analysis & Business Insights
- Formulated and executed complex SQL queries to derive business insights, such as:
  - **Sales and Payment Insights**
    ```sql
    SELECT PAYMENT_METHOD, COUNT(*) AS NO_OF_TXN, SUM(QUANTITY) AS NO_OF_QTY_SOLD 
    FROM WALMART 
    GROUP BY PAYMENT_METHOD;
    ```
  - **Branch Performance Analysis**
    ```sql
    SELECT BRANCH, CATEGORY, AVG(RATING) AS AVG_RATING 
    FROM WALMART 
    GROUP BY BRANCH, CATEGORY
    ORDER BY AVG_RATING DESC;
    ```
  - **Peak Shopping Hours and Busiest Days**
    ```sql
    SELECT BRANCH, DAYNAME(DATE) AS DAY, COUNT(*) AS NO_OF_TXN 
    FROM WALMART 
    GROUP BY BRANCH, DAYNAME(DATE)
    ORDER BY NO_OF_TXN DESC;
    ```
  - **Revenue Trends and Profitability**
    ```sql
    SELECT CATEGORY, SUM(TOTAL_PRICE) AS TOTAL_REVENUE, SUM((UNIT_PRICE * PROFIT_MARGIN) * QUANTITY) AS TOTAL_PROFIT 
    FROM WALMART 
    GROUP BY CATEGORY 
    ORDER BY TOTAL_PROFIT DESC;
    ```
  - **Year-over-Year Revenue Change**
    ```sql
    WITH REVENUE_2022 AS (SELECT BRANCH, SUM(TOTAL_PRICE) AS REVENUE FROM WALMART WHERE YEAR(DATE) = 2022 GROUP BY BRANCH),
    REVENUE_2023 AS (SELECT BRANCH, SUM(TOTAL_PRICE) AS REVENUE FROM WALMART WHERE YEAR(DATE) = 2023 GROUP BY BRANCH)
    SELECT LYS.BRANCH, LYS.REVENUE AS LAST_YEAR_REVENUE, CYS.REVENUE AS CURRENT_YEAR_REVENUE,
    (CYS.REVENUE - LYS.REVENUE) AS REVENUE_CHANGE, 
    CONCAT(ROUND(((CYS.REVENUE - LYS.REVENUE) / LYS.REVENUE) * 100, 2), '%') AS PERCENT_DECREASED
    FROM REVENUE_2022 AS LYS JOIN REVENUE_2023 AS CYS 
    ON LYS.BRANCH = CYS.BRANCH 
    WHERE LYS.REVENUE > CYS.REVENUE 
    ORDER BY REVENUE_CHANGE ASC LIMIT 5;
    ```

## Results and Insights
- **Sales Insights**: Identified top-selling categories, busiest branches, and the most frequently used payment methods.
- **Profitability**: Analyzed top revenue-generating product categories and branches.
- **Customer Behavior**: Evaluated rating trends, payment preferences, and peak shopping hours.

## Future Enhancements
- **Interactive Data Visualization**: Implement Power BI or Tableau for dashboarding.
- **Real-time Data Pipeline**: Automate data ingestion for continuous monitoring.
- **Predictive Analytics**: Use machine learning to forecast sales trends and customer behavior.

## Conclusion
This project provides a structured, data-driven approach to analyzing Walmart's sales performance. By leveraging Python, SQL, and advanced analytics, it delivers valuable insights that can help drive business decisions and optimize sales strategies.



## Acknowledgments

- **Data Source**: Kaggle’s Walmart Sales Dataset
- **Inspiration**: Walmart’s business case studies on sales and supply chain optimization.

---
