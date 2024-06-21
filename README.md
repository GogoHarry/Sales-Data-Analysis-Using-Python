# Sales-Data-Analysis-Using-Python

## Project Overview
The goal of this project is to analyze the historical sales data of XYZ Company to identify key factors influencing sales performance. This analysis will provide insights and recommendations to enhance sales revenue and profitability.

## Table of Contents
1. [Project Overview](#project-overview)
2. [Data Source](#data-source)
3. [Tools](#tools)
4. [Data Description](#data-description)
5. [Descriptive Analysis](#descriptive-analysis)
6. [Data Cleaning](#data-cleaning)
7. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)
   - [Time Series Analysis](#time-series-analysis) 
   - [Customer Analysis](#customer-analysis)
   - [Product Analysis](#product-analysis)
   - [Payment Type Analysis](#payment-type-analysis)
   - [Correlation Analysis](#correlation-analysis)
9. [Business Recommendations](#business-recommendations)
10. [Future Work](#future-work)
11. [References](#references)

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

### Descriptive Analysis

Summary statistics for numerical columns and frequency distribution for categorical columns.

```python
# Descriptive Statistics
summary_stats = sales_data.describe(include='all')
summary_stats
```

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

Exploratory Data Analysis (EDA) is a crucial step in any data analysis project. It involves summarizing the main characteristics of the data and visualizing them to gain insights. 

For this project, we will analyze the sales data and answer key questions, such as:

- Time Series Analysis
  - Analyze sales trends over time.
    - How do sales vary over time?
```python
# Group the data by year and sum the sales
sales_month = sales_data.groupby(sales_data['timestamp'].dt.day)['sales'].sum()

# Create the figure and axis
fig, ax = plt.subplots(figsize=(12, 6))

# Plot the line chart
ax.plot(sales_month.index, sales_month.values, color='skyblue', marker='o')

# Add labels and title
ax.set_title('Sales Trend Over Time', fontsize=16)
ax.set_xlabel('Timestamps', fontsize=14)
ax.set_ylabel('Total Sales', fontsize=14)

# Add grid lines
#ax.grid(linestyle='-', alpha=0.5)

# Format the x-axis ticks as integers
ax.xaxis.set_major_formatter('{x:.0f}')

# Add data labels
for x, y in zip(sales_month.index, sales_month.values):
    ax.annotate(f'{y:,.0f}', (x, y), textcoords='offset points', xytext=(0, 10), ha='center')

# Adjust the spacing
plt.subplots_adjust(left=0.1, right=0.95, top=0.9, bottom=0.15)

# Display the plot
plt.show()
```

![image](https://github.com/GogoHarry/Sales-Data-Analysis-Using-Python/assets/82883963/883f180f-6cab-491f-ad1f-26e40c8d2cef)

As part of our EDA, we will explore the factors influencing sales

- Customer Analysis:
  - Analyze sales performance by customer type.
    - How do sales differ among customer types (e.g., gold, standard, premium, basic)?

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

![image](https://github.com/GogoHarry/Sales-Data-Analysis-Using-Python/assets/82883963/de97309a-776a-4e14-a0c3-881a3456123d)


- Product Analysis:
  - Analyze sales performance by product and category.
    - Which product categories have the highest sales? How do individual products perform?
```python
# Aggregate sales by category
category_sales = sales_data.groupby('category')['sales'].sum().reset_index()
category_sales
```
```python
# Visualising
plt.figure(figsize=(14, 6))
sns.barplot(data=category_sales, x='category', y='sales')
plt.title('Sales by Category')
plt.xlabel('Category')
plt.ylabel('Total Sales')
plt.xticks(rotation=45, ha='right')
plt.show()
```

![image](https://github.com/GogoHarry/Sales-Data-Analysis-Using-Python/assets/82883963/c62840c3-bfc5-4986-af6b-9e0e175990ba)


- Payment Type Analysis:
  - Investigate the impact of payment types on sales.
    - How do different payment methods (e.g., e-wallet, debit card, cash) affect sales?
```python
# Aggregate sales by payment type
sales_by_payment_method = sales_data.groupby('payment_type')['sales'].sum().reset_index()
sales_by_payment_method
```
  
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Example data for sales by payment method
sales_by_payment_method = {
    'payment_type': ['Credit Card', 'Debit Card', 'Cash', 'E-wallet'],
    'sales': [39309.52, 37010.67, 41287.31, 36701.35]
}
sales_by_payment_method = pd.DataFrame(sales_by_payment_method)

# Set the style for the plot
sns.set_style('darkgrid')

# Pie chart customization variables
explode = (0.1, 0, 0, 0)  # Explode the first slice
colors = ['#4CAF50', '#2196F3', '#FF9800', '#F44336']  # Different colors for each slice

# Autopct formatting function
def autopct_format(values):
    def my_format(pct):
        total = sum(values)
        val = int(round(pct*total/100.0))
        return '{p:.2f}%\n({v:d})'.format(p=pct,v=val)
    return my_format

# Creating the pie chart
plt.figure(figsize=(10, 5))
plt.pie(sales_by_payment_method['sales'], explode=explode, labels=sales_by_payment_method['payment_type'], colors=colors, autopct=autopct_format(sales_by_payment_method['sales']), startangle=90, textprops={'fontsize': 14})
plt.title('Sales by Payment Type', fontsize=18)
plt.axis('equal')
plt.show()
```


![image](https://github.com/GogoHarry/Sales-Data-Analysis-Using-Python/assets/82883963/be6cbff6-35e8-40db-901e-d30355786e0b)


- Correlation Analysis:
  - Identify correlations between different variables.
    - What are the relationships between numerical variables (e.g., unit price, quantity, total sales)?
```python
# Correlation matrix
numerical_data = sales_data.select_dtypes(include=['int64', 'float64'])                                                      
correlation_matrix = numerical_data.corr()

# Plot correlation matrix
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', fmt=".2f")
plt.title('Correlation Matrix of Numerical Variables')
plt.show()
```
![image](https://github.com/GogoHarry/Sales-Data-Analysis-Using-Python/assets/82883963/13298de8-47ed-4c0a-a9fa-a190e5b3e847)


### Key Result and Findings

1. **Time Series Analysis**
   - **Daily Sales Patterns:**
     - Sales fluctuate noticeably on different days. Specific days of the week may show higher sales, suggesting the influence of weekly shopping habits or promotional events.
   - Hourly Sales Distribution:
     - There are clear peaks and troughs in sales at different times of the day. Peak sales hours are identified, which can help optimize staffing and inventory during these times.
2. **Customer Type Analysis**
   - **Non-Member Sales Dominance:**
     - Among all customer types, non-members generate the highest total sales. This suggests that non-members are a significant customer segment and contribute substantially to overall revenue.
   - **Comparable Performance:**
     - The sales figures for the basic, gold, premium, and standard customer types are fairly close to each other. This indicates a balanced contribution from these segments, with no single membership tier vastly outperforming or underperforming the others.
   - **Potential Growth in Membership Tiers:**
     - Given the high sales from non-members, there might be an opportunity to convert these non-members into members by offering attractive incentives or benefits. This could lead to increased loyalty and potentially higher sales.3. Product Analysis
   - Sales vary significantly across different product categories. Categories like kitchen, meat, and frozen products are top performers with higher sales volumes. Categories such as spices and herbs, pet food, and snacks have lower sales, indicating potential areas for improvement or reevaluation.
   - **High-Performing Categories:**
     - The highest sales are seen in the kitchen and meat categories, suggesting these are core products for the company. The performance of these categories might be leveraged for targeted promotions and stocking decisions.
   - **Low-Performing Categories:**
     - Categories with lower sales may require further analysis to understand the underlying reasons, such as lack of promotion, lower demand, or higher competition. Strategies such as product bundling, discounts, or enhanced marketing could be considered to boost sales in these areas
4. **Payment type analysis**
   - **Balanced Distribution Across Payment Types:**
     - The sales distribution across different payment types (Credit Card, Debit Card, E-wallet, and Cash) is relatively balanced, with each type contributing a significant portion of the total sales.
   - **Cash Dominates Slightly:**
     - Cash payments account for the largest share of sales at 26.76% (41,287 sales). This indicates that a substantial number of customers prefer to pay with cash.
   - **Credit Card Usage is Prominent:**
     - Credit Card payments are also highly popular, accounting for 25.47% (39,310 sales) of the total sales. This suggests that many customers find credit cards a convenient payment method.
   - **E-wallet and Debit Card Usage:**
     - E-wallet (23.78%, 36,701 sales) and Debit Card (23.98%, 37,011 sales) payments have a comparable share, indicating a growing trend of digital and card-based transactions.
5. **Correlation Insights:**
   - There is a strong positive correlation between unit_price and sales suggesting that the unit price is a significant factor influencing total sales. Since unit price strongly influences total sales, optimizing pricing strategies might be an effective way to boost sales revenue.

### Business Recommendations

1. **Optimize Sales Timing and Staffing**
   - **Daily Sales Patterns**:
     - Identify days with the highest sales and align promotions, discounts, and special events on these days to maximize sales.
     - For days with lower sales, implement targeted marketing campaigns to drive traffic.
   - **Hourly Sales Distribution**:
     - Increase staffing during peak sales hours to improve customer service and reduce wait times.
     - Schedule inventory restocking during off-peak hours to ensure product availability without disrupting customer experience.

2. **Enhance Customer Membership Programs**
   - **Non-Member Sales Dominance**:
     - Develop marketing strategies to convert non-members into members by offering exclusive benefits such as discounts, loyalty points, or special promotions.
     - Create targeted communication campaigns to highlight the advantages of membership and encourage sign-ups.
   - **Comparable Performance Among Membership Tiers**:
     - Analyze and understand the unique needs and preferences of each membership tier to tailor marketing efforts.
     - Offer tier-specific promotions to incentivize purchases and increase overall sales across all membership levels.

3. **Product Category Management**
   - **High-Performing Categories**:
     - Focus on maintaining high stock levels for top-performing categories like kitchen and meat to meet demand and avoid stockouts.
     - Leverage the popularity of these categories in marketing campaigns and cross-promotions with other products.
   - **Low-Performing Categories**:
     - Conduct a detailed analysis of low-performing categories (e.g., spices and herbs, pet food, snacks) to identify the reasons for lower sales.
     - Consider product bundling, discounts, or enhanced marketing efforts to boost sales in these categories.
     - Evaluate the possibility of phasing out underperforming products and replacing them with new, in-demand items.

4. **Payment Method Optimization**
   - **Balanced Distribution Across Payment Types**:
     - Ensure all payment options (Credit Card, Debit Card, E-wallet, and Cash) are seamlessly integrated and functional at all times to accommodate customer preferences.
     - Promote digital payment methods (E-wallet and Debit Card) by offering exclusive discounts or rewards for using these options.
   - **Cash Dominates Slightly**:
     - Maintain efficient cash handling processes and ensure adequate cash registers and change availability during peak hours to avoid delays.
     - Educate customers on the benefits of digital payments to gradually shift preferences and reduce cash handling costs.

5. **Pricing Strategy and Sales Correlation**
   - **Strong Positive Correlation Between Unit Price and Sales**:
     - Implement a dynamic pricing strategy to optimize revenue, adjusting prices based on demand, competition, and customer behavior.
     - Consider premium pricing for high-demand products to maximize sales revenue.
   - **Minimal Correlation Between Quantity and Sales**:
     - Focus on marketing strategies that highlight product value and benefits rather than relying solely on bulk sales.
     - Offer value-added services or product enhancements to justify higher prices and drive sales revenue.

By implementing these recommendations, XYZ Company can effectively leverage the insights gained from the sales data analysis to enhance operational efficiency, improve customer satisfaction, and drive overall business growth.

### Data Limitation
1. **Limited Time Frame**
  - **Seasonality and Trends:** The dataset covers only 7 days in March 2022, which may not capture the full range of seasonal patterns, trends, or anomalies. Sales data over a longer period is necessary to identify consistent patterns and trends.
  - **Special Events:** The selected week may include or exclude special events (e.g., holidays, promotions) that could significantly impact sales, leading to skewed analysis.
2. **Sample Size**
  - **Representativeness:** A week's worth of data may not be representative of the overall sales performance throughout the year. Larger datasets provide a more accurate and reliable analysis.
  - **Variability:** Short-term data can be heavily influenced by random fluctuations or outliers that may not reflect typical business operations.

### Future Work
1. **Advanced Customer Segmentation**
- Conduct a detailed segmentation analysis to identify distinct customer groups based on purchasing behavior, demographics, and preferences.
- Develop personalized marketing strategies and promotions tailored to each segment to enhance customer engagement and loyalty.
2. **Predictive Analytics for Sales Forecasting**
- Implement machine learning models to predict future sales trends based on historical data, seasonal patterns, and external factors.
- Use these forecasts for better inventory management, staffing optimization, and strategic planning.

### References

