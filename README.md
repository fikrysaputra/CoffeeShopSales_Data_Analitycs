# CoffeeShopSales Data Analitycs
Transaction records for Maven Roasters, a fictitious coffee shop operating out of three NYC locations. Dataset includes the transaction date, timestamp and location, along with product-level details. Dataset resorce : https://www.kaggle.com/datasets/ahmedabbas757/coffee-sales

# Data Preview
transaction_id : Unique sequential ID representing an individual transaction

transaction_date : Date of the transaction (MM/DD/YY)

transaction_time : Timestamp of the transaction (HH:MM:SS)

transaction_qty : Quantity of items sold

store_id : Unique ID of the coffee shop where the transaction took place

store_location : Location of the coffee shop where the transaction took place

product_id : Unique ID of the product sold

unit_price : Retail price of the product sold

product_category : Description of the product category

product_type : Description of the product type

product_detail : Description of the product detail

# Check Null Data
| Column              | Null Count |
|---------------------|------------|
| transaction_id      | 0          |
| transaction_date    | 0          |
| transaction_time     | 0          |
| transaction_qty     | 0          |
| store_id            | 0          |
| store_location      | 0          |
| product_id         | 0          |
| unit_price          | 0          |
| product_category    | 0          |
| product_type       | 0          |
| product_detail      | 0          |

dtype: int64

# Check Duplicated
df_coffee.duplicated().sum()

0

# Describe
df_coffee.describe()
| Statistic       | transaction_id | transaction_qty | store_id | product_id | unit_price |
|------------------|----------------|------------------|----------|------------|------------|
| count            | 149116.000000  | 149116.000000    | 149116.000000 | 149116.000000 | 149116.000000 |
| mean             | 74737.371872   | 1.438276         | 5.342063 | 47.918607  | 3.382219   |
| std              | 43153.600016   | 0.542509         | 2.074241 | 17.930020  | 2.658723   |
| min              | 1.000000       | 1.000000         | 3.000000 | 1.000000   | 0.800000   |
| 25%              | 37335.750000   | 1.000000         | 3.000000 | 33.000000  | 2.500000   |
| 50%              | 74727.500000   | 1.000000         | 5.000000 | 47.000000  | 3.000000   |
| 75%              | 112094.250000  | 2.000000         | 8.000000 | 60.000000  | 3.750000   |
| max              | 149456.000000  | 8.000000         | 8.000000 | 87.000000  | 45.000000  |

# Dataframe info
RangeIndex: 149116 entries, 0 to 149115
Data columns (total 11 columns):
| Column             | Non-Null Count   | Dtype          |
|---------------------|------------------|----------------|
| transaction_id      | 149116 non-null   | int64          |
| transaction_date    | 149116 non-null   | datetime64[ns] |
| transaction_time     | 149116 non-null   | object         |
| transaction_qty     | 149116 non-null   | int64          |
| store_id           | 149116 non-null   | int64          |
| store_location      | 149116 non-null   | object         |
| product_id         | 149116 non-null   | int64          |
| unit_price         | 149116 non-null   | float64        |
| product_category    | 149116 non-null   | object         |
| product_type       | 149116 non-null   | object         |
| product_detail      | 149116 non-null   | object         |

dtypes: datetime64[ns](1), float64(1), int64(4), object(5)

memory usage: 12.5+ MB

# Time Range
```python
df_coffee['transaction_date'] = pd.to_datetime(df_coffee['transaction_date'])

# Find the date and time range
min_date = df_coffee['transaction_date'].min()
max_date = df_coffee['transaction_date'].max()
min_time = df_coffee['transaction_time'].min()
max_time = df_coffee['transaction_time'].max()

print("Date Range:", min_date, "to", max_date)
print("Time Range:", min_time, "to", max_time)
```
Date Range: 2023-01-01 00:00:00 to 2023-06-30 00:00:00

Time Range: 06:00:00 to 20:59:32

# Extract time
```python
df_coffee['transaction_date'] = pd.to_datetime(df_coffee['transaction_date'])
df_coffee['transaction_time'] = pd.to_datetime(df_coffee['transaction_time'], format='%H:%M:%S').dt.time
df_coffee['month'] = df_coffee['transaction_date'].dt.month
df_coffee['year'] = df_coffee['transaction_date'].dt.year
df_coffee['day_of_week'] = df_coffee['transaction_date'].dt.day_name()
df_coffee['hour'] = pd.to_datetime(df_coffee['transaction_time'].astype(str), format='%H:%M:%S').dt.hour
df_coffee['month_name'] = pd.to_datetime(df_coffee['month'], format='%m').dt.month_name()
```

# Exploratory Data Analysis
# Sales over time
```python
# Total earnings product sales in every month

total_sales_by_month = df_coffee.groupby(df_coffee['transaction_date'].dt.month)['total_sales'].sum().reset_index()
total_sales_by_month.columns = ['Month', 'Total Earnings']
total_sales_by_month['Month'] = pd.to_datetime(total_sales_by_month['Month'], format='%m').dt.month_name()
total_sales_by_month
```
The Latest Month (June) total earnings **166485.88**

**Total earning each month :**
| Month     | Total Product Earnings |
|-----------|-----------------------|
| January   | 81677.74              |
| February  | 76145.19              |
| March     | 98834.68              |
| April     | 118941.08             |
| May       | 156727.76             |
| June      | 166485.88             |

**Total earnings each store location per month:**
| Store Location      | April     | February  | January   | June      | March     | May       |
|---------------------|-----------|------------|-----------|-----------|-----------|-----------|
| Astoria             | 39477.61  | 25105.34   | 27313.66  | 55083.11  | 32835.43  | 52428.76  |
| Hell's Kitchen      | 40304.14  | 25719.80   | 27820.65  | 56957.08  | 33110.57  | 52598.93  |
| Lower Manhattan      | 39159.33  | 25320.05   | 26543.43  | 54445.69  | 32888.68  | 51700.07  |

**Average Daily sales each store location:**
| Store Location      | Month     | Average Daily Sales |
|---------------------|-----------|---------------------|
| Astoria             | January   | 267.13              |
| Astoria             | February  | 275.64              |
| Astoria             | March     | 322.55              |
| Astoria             | April     | 400.87              |
| Astoria             | May       | 519.81              |
| Astoria             | June      | 561.77              |
| Hell's Kitchen      | January   | 269.03              |
| Hell's Kitchen      | February  | 283.46              |
| Hell's Kitchen      | March     | 328.97              |
| Hell's Kitchen      | April     | 406.47              |
| Hell's Kitchen      | May       | 514.32              |
| Hell's Kitchen      | June      | 570.80              |
| Lower Manhattan      | January   | 266.10              |
| Lower Manhattan      | February  | 281.96              |
| Lower Manhattan      | March     | 329.32              |
| Lower Manhattan      | April     | 408.30              |
| Lower Manhattan      | May       | 521.77              |
| Lower Manhattan      | June      | 565.50              |

**Average daily sales :**
| Store Location      | Average Daily Sales |
|---------------------|---------------------|
| Astoria             | 1283.12             |
| Hell's Kitchen      | 1306.69             |
| Lower Manhattan      | 1271.03             |

**Overall Average Daily Sales:** $1286.95

**Total Product Sales by Transaction Month**
![download](https://github.com/user-attachments/assets/7f748a4d-61e0-4135-ab44-7672532c31e6)

**Total Units Sold Every Day**
![download](https://github.com/user-attachments/assets/bb5532b6-7e4a-4a3d-aedf-c668007f5d19)

**Total Product Sales by Month**
![download](https://github.com/user-attachments/assets/e10d616f-a118-4006-8e31-159ff0942b70)

**Total Product Sales by Month**
![download](https://github.com/user-attachments/assets/b58cd72f-e59c-4f2d-8e49-1f7a8f58564f)

**We can see :**
1. Total earnings is always raise every month
2. Each Store Location didnt have gap earnings too far (perform similarly)
3. Data indicate have potential seasonal trends in sales, need strategic for marketing during those time

# Product Total Sales
**Most Expensive Product**
```python
most_expensive_product = df_coffee.loc[df_coffee['unit_price'].idxmax()]

# Calculate total sales and total quantity for the most expensive product
total_sales = df_coffee[df_coffee['product_id'] == most_expensive_product['product_id']]['total_sales'].sum()
total_qty = df_coffee[df_coffee['product_id'] == most_expensive_product['product_id']]['transaction_qty'].sum()

output = f"""
Highest Price: {most_expensive_product['unit_price']}
Product ID: {most_expensive_product['product_id']}
Total Earnings: {total_sales}
Total Sales unit: {total_qty}
Product Category: {most_expensive_product['product_category']}
Product Type: {most_expensive_product['product_type']}
Product Detail: {most_expensive_product['product_detail']}
"""
print(output)
```

Highest Price: 45.0

Product ID: 8

Total Earnings: 11700.0

Total Sales unit: 260

Product Category: Coffee beans

Product Type: Premium Beans

Product Detail: Civet Cat

**Most Cheap Product**
Cheap Price: 0.8

Product ID: 64

Total Earnings: 1897.6

Total Sales unit: 2372

Product Category: Flavours

Product Type: Regular syrup

Product Detail: Hazelnut syrup

**Highest unit sales**
Highest Sales unit: 4708

Price: 2.5

Product ID: 50

Total Earnings: 11770.0

Product Category: Tea

Product Type: Brewed Black tea

Product Detail: Earl Grey Rg

**Lowest unit sales**
Lowest Sales unit: 2

Price: 4.69

Product ID: 73

Total Earnings: 9.38

Product Category: Bakery

Product Type: Pastry

Product Detail: Almond Croissant

**Highest earnings product**
Sales unit: 4453

Price: 4.75

Product ID: 61

Total Earnings: 21151.75

Product Category: Drinking Chocolate

Product Type: Hot chocolate

Product Detail: Sustainably Grown Organic Lg

**Total unit sold for Sustainably Grown Organic Lg**
Sustainably Grown Organic Lg is the most highest total earnings
![download](https://github.com/user-attachments/assets/f011cdba-2e97-4fa3-ae01-a0189724a026)

**Total unit sold over time for Civet Cat**
![download](https://github.com/user-attachments/assets/2f7a02a1-9e25-43a9-be28-3191eacbcbf6)

**Total unit sold over time for Earl Grey Rg**
![download](https://github.com/user-attachments/assets/9d888a86-a3a1-4d6a-ae8c-6385f4aea92b)

**Top 10 Product Detail**
![download](https://github.com/user-attachments/assets/906791fe-78e6-4f0b-ace6-646f2740393f)


**Top 5 Product Type**
![download](https://github.com/user-attachments/assets/273587e8-e3ea-4256-b7f9-cc8a71574793)

**Top 5 Product Categories**
![download](https://github.com/user-attachments/assets/fa1104b1-17b7-4bb6-9813-1c4ec646bd5b)

**Insight**
This analysis highlights key products within the Maven Roasters coffee shop dataset, emphasizing the most and least expensive products, sales performance, and product categorization.

1. The Civet Cat is the most expensive product, reflecting a high unit price and generating significant total earnings, indicating strong demand despite its price point.
2. Hazelnut Syrup has achieved notable unit sales, suggesting a steady demand for flavored syrups in coffee drinks.
3. Earl Grey Rg, with the highest unit sales, demonstrates that more affordable products can drive significant volume, contributing meaningfully to overall earnings.
4. The Almond Croissant's low sales indicate limited interest or a niche market, warranting further exploration into customer preferences for bakery items.
5. Sustainably Grown Organic Lg stands out with the highest total earnings, underscoring the value of quality and sustainability in consumer choices.
  
# Time Transacion

**Total earnings by hour**
![download](https://github.com/user-attachments/assets/d8cbed66-90c5-4f66-a6bf-790899c2f78b)

**Transaction hour**
![download](https://github.com/user-attachments/assets/1f47460f-5f7d-4229-8403-c5088a57e922)

**Average Transaction**
Average Transaction Time: 12:14:15

by location

Store Location: **Astoria**

Average Transaction Time: 13:02:39

Store Location: **Hell's Kitchen**

Average Transaction Time: 12:11:55

Store Location: **Lower Manhattan**

Average Transaction Time: 11:25:29

**Transaction each store location**
![download](https://github.com/user-attachments/assets/24e80a73-b498-4fe6-a36f-fb2d4107b263)

**Average transaction by day of week**
![download](https://github.com/user-attachments/assets/c6460457-d89d-4a31-94dd-f35c6111e8dc)

**Insight**
1. Friday is most busy time, Fridays generate the highest volume of transactions
2. Each store location shows varying average transaction times, with Astoria having the longest average. This may indicate customer flow and service efficiency differences.
3. 8-10 in the morning is highest people do transaction


# Conclusion
The analysis of the Maven Roasters coffee shop sales data provides valuable insights into transaction patterns, product performance, and customer behavior across three locations in New York City. Key findings include:

**Sales Growth:** The coffee shop has experienced consistent monthly growth in total earnings from January to June, indicating a positive trend in sales and customer engagement.

**Location Performance:** Each store location shows similar earnings patterns, with no significant gaps in performance. Astoria consistently leads in average daily sales, but all locations have potential for growth, suggesting effective management across the board.

**Product Insights:**

**1**The Civet Cat, although expensive, generates substantial earnings, reflecting its appeal among consumers despite the high price point.

**2**The Hazelnut Syrup demonstrates strong demand as a lower-priced item, indicating that affordable products can also drive significant sales volume.

**3**Earl Grey Rg stands out with the highest unit sales, showcasing the 
importance of value in consumer choices.

**4**Conversely, the Almond Croissant has low sales, suggesting it may not align with customer preferences, which could be an area for potential adjustments in inventory or marketing.

**5**Transaction Timing: The busiest times for transactions are notably on Fridays, particularly between 8 AM and 10 AM, highlighting opportunities for targeted marketing or promotions during peak hours. The average transaction times vary by location, with Astoria taking the longest, indicating potential for operational improvements in service efficiency.

**Seasonal Trends**: The analysis suggests potential seasonal trends that could inform strategic marketing efforts. Understanding these patterns can help Maven Roasters optimize inventory, staffing, and promotions to better meet customer demand.

In summary, this data analysis highlights areas for growth, opportunities for operational improvements, and insights into consumer preferences. By leveraging this information, Maven Roasters can enhance its marketing strategies, improve customer satisfaction, and drive sales growth in the competitive coffee shop market.
