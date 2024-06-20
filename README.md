# Sales-Data-Analysis-Using-Python

## Project Overview
The goal of this project is to analyze the historical sales data of XYZ Company to identify key factors influencing sales performance. This analysis will provide insights and recommendations to enhance sales revenue and profitability.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Data Source](#data-source)
3. [Tools](#tools)
4. [Data Description](#data-description)
5. [Data Cleaning](#data-cleaning)
6. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
   - [Descriptive Analysis](#descriptive-analysis)
   - [Time Series Analysis](#time-series-analysis)
7. [Factors Influencing Sales](#factors-influencing-sales)
   - [Customer Analysis](#customer-analysis)
   - [Product Analysis](#product-analysis)
   - [Payment Type Analysis](#payment-type-analysis)
   - [Correlation Analysis](#correlation-analysis)
8. [Conclusion and Recommendations](#conclusion-and-recommendations)
9. [Future Work](#future-work)
10. [References](#references)

## Data Source
The primary dataset used for this analysis is XYZ's company historical sample sales data.csv file, containing detailed information about each transaction made by the company.

## Tools
- Python - Data cleaning, analysis, and visualisation
  - [Install here](https://www.anaconda.com/download/success)

## Data Description

The dataset contains transaction records from XYZ Company. The key columns in the dataset are:

- `transaction_id`: Unique identifier for each transaction.
- `timestamp`: Date and time of the transaction.
- `product_id`: Unique identifier for each product.
- `category`: Product category.
- `customer_type`: Type of customer (e.g., gold, standard, premium, basic).
- `unit_price`: Price per unit of the product.
- `quantity`: Quantity of the product purchased.
- `total`: Total amount for the transaction.
- `payment_type`: Payment method (e.g., e-wallet, debit card).

## Data Cleaning
Data cleaning steps carried out involved:

1. Checking for missing values: No missing values were found.
2. Checking for duplicates: No duplicate rows found.

```python
# import the necessary libraries
import numpy as np
import pandas as pd

# Load the data
sales_data = pd.read_csv('sample_sales_data.csv')

# Check for missing values
missing_values = sales_data.isnull().sum()

# Check for duplicates
duplicates = sales_data.duplicated().sum()

missing_values, duplicates
```
### Data Preprocessing
```python
# Drop 'unnamed' column and rename 'total' to 'sales'

sales_data.drop('Unnamed: 0', axis = 1, inplace = True)

sales_data.rename(columns = {'total': 'sales'}, inplace = True)

# view few rows
sales_data.head()
```
```python
# Convert the 'timestamp' to datetime data type

sales_data['timestamp'] = pd.to_datetime(sales_data['timestamp'])

# check the info
sales_data.info()
```

### Data Validation
```python
# Extract the numerical columns

num_vars = sales_data.select_dtypes(include = ['float', 'int']).columns.tolist()

num_vars
```
```python
# Value counts for the numerical columns

for column in num_vars:
  print(sales_data[column].value_counts())
```
```python
# Extract the categorical columns

cat_vars = sales_data.select_dtypes(include = ['object', 'category']).columns.tolist()
cat_vars
```
```python
# Value counts for the categorical columns
for column in cat_vars:
  print(sales_data[column].value_counts())
```

### Exploratory Data Analysis (EDA)
-  Descriptive Analysis

Summary statistics for numerical columns and frequency distribution for categorical columns.
```python
# Descriptive Statistics
summary_stats = sales_data.describe(include='all')
summary_stats
```

Exploratory Data Analysis (EDA) is a crucial step in any data analysis project. It involves summarizing the main characteristics of the data and visualizing them to gain insights. For this project, we will explore the sales data and answer key questions, such as:

- Time Series Analysis
  - Analyze sales trends over time.

How do sales vary over different time periods (daily, monthly, hourly)?


- Customer Analysis
  - Analyze sales performance by customer type.
Question: How do sales differ among different customer types (e.g., gold, standard, premium, basic)?

```python
# Aggregate sales by customer type

data=sales_by_customer_type = sales_data.groupby('customer_type')['sales'].sum().reset_index()

# Plot customer type sales
plt.figure(figsize=(10, 5))
sns.barplot(data=sales_by_customer_type, x='customer_type', y='sales')
plt.title('Sales by Customer Type')
plt.xlabel('Customer Type')
plt.ylabel('Total Sales')
plt.show()
```

- Product Analysis
- 
Step: Analyze sales performance by product and category.
Question: Which product categories have the highest sales? How do individual products perform?
Visualizations: Bar plots showing total sales by product category.

- Payment Type Analysis

Step: Investigate the impact of payment types on sales.
Question: How do different payment methods (e.g., e-wallet, debit card, cash) affect sales?
Visualizations: Bar plots showing total sales by payment type.

- Correlation Analysis

Step: Identify correlations between different variables.
Question: What are the relationships between different numerical variables (e.g., unit price, quantity, total sales)?
Visualizations: Heatmap of the correlation matrix.



