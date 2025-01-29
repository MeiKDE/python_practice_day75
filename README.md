# Google Trends Data Analysis

## Introduction
This project explores whether Google Trends search volume correlates with financial and economic data. We analyze search trends for Bitcoin, Tesla, and "Unemployment Benefits" to see how they compare with respective stock prices and unemployment rates.

### **Data Sources:**
- [Unemployment Rate from FRED](https://fred.stlouisfed.org/series/UNRATE/)
- [Google Trends](https://trends.google.com/trends/explore)
- [Yahoo Finance for Tesla Stock Price](https://finance.yahoo.com/quote/TSLA/history?p=TSLA)
- [Yahoo Finance for Bitcoin Stock Price](https://finance.yahoo.com/quote/BTC-USD/history?p=BTC-USD)

---

## **Installation & Dependencies**
Make sure you have the required Python libraries installed:
```bash
pip install pandas matplotlib
```

Import necessary libraries:
```python
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as mdates
```

---

## **Data Exploration**
### **Loading Data**
Download the `.csv` files and place them in the working directory.
```python
df_tesla = pd.read_csv('TESLA Search Trend vs Price.csv')
df_btc_search = pd.read_csv('Bitcoin Search Trend.csv')
df_btc_price = pd.read_csv('Daily Bitcoin Price.csv')
df_unemployment = pd.read_csv('UE Benefits Search vs UE Rate 2004-19.csv')
```

### **Inspecting the Data**
```python
print(df_tesla.shape)  # Check the number of rows and columns
print(df_tesla.columns) # Check column names
print(df_tesla.describe()) # Get summary statistics
```

---

## **Data Cleaning**
### **Checking for Missing Values**
```python
print(df_tesla.isnull().sum())
df_tesla.dropna(inplace=True)  # Remove missing values
```

### **Convert Strings to Datetime**
```python
df_tesla['MONTH'] = pd.to_datetime(df_tesla['MONTH'])
df_btc_search['MONTH'] = pd.to_datetime(df_btc_search['MONTH'])
df_btc_price['DATE'] = pd.to_datetime(df_btc_price['DATE'])
df_unemployment['MONTH'] = pd.to_datetime(df_unemployment['MONTH'])
```

### **Resampling Data (Convert Daily to Monthly)**
```python
df_btc_monthly = df_btc_price.resample('M', on='DATE').mean()
```

---

## **Data Visualization**
### **Tesla Stock Price vs. Search Volume**
```python
plt.figure(figsize=(14,8), dpi=120)
plt.title('Tesla Web Search vs Price', fontsize=18)

ax1 = plt.gca()
ax2 = ax1.twinx()

ax1.set_ylabel('TSLA Stock Price', color='g', fontsize=14)
ax2.set_ylabel('Search Trend', color='b', fontsize=14)

ax1.plot(df_tesla.MONTH, df_tesla.TSLA_USD_CLOSE, color='g', linewidth=3)
ax2.plot(df_tesla.MONTH, df_tesla.TSLA_WEB_SEARCH, color='b', linewidth=3)

plt.show()
```

### **Bitcoin Price vs. Search Volume**
```python
plt.figure(figsize=(14,8), dpi=120)
plt.title('Bitcoin News Search vs Resampled Price', fontsize=18)

ax1 = plt.gca()
ax2 = ax1.twinx()

ax1.set_ylabel('Bitcoin Price', color='g', fontsize=14)
ax2.set_ylabel('Search Trend', color='b', fontsize=14)

ax1.plot(df_btc_monthly.index, df_btc_monthly.CLOSE, color='g', linewidth=3, linestyle='--')
ax2.plot(df_btc_monthly.index, df_btc_search.BTC_NEWS_SEARCH, color='b', linewidth=3, marker='o')

plt.show()
```

### **Unemployment Benefits Search vs. Unemployment Rate**
```python
plt.figure(figsize=(14,8), dpi=120)
plt.title('Monthly Search of "Unemployment Benefits" in the U.S. vs the Unemployment Rate', fontsize=18)

ax1 = plt.gca()
ax2 = ax1.twinx()

ax1.set_ylabel('FRED U/E Rate', color='g', fontsize=14)
ax2.set_ylabel('Search Trend', color='b', fontsize=14)

ax1.plot(df_unemployment.MONTH, df_unemployment.UNRATE, color='g', linewidth=3, linestyle='--')
ax2.plot(df_unemployment.MONTH, df_unemployment.UE_BENEFITS_WEB_SEARCH, color='b', linewidth=3)

plt.show()
```

---

## **Findings & Insights**
- **Tesla:** Search trends often spike during major product launches or significant stock price changes.
- **Bitcoin:** Search trends tend to correlate with high price volatility.
- **Unemployment Benefits:** Searches tend to **lead** the unemployment rate, indicating potential predictive power.

---

## **Credits**
This project is based on the **100 Days of Code: Python Bootcamp** by [Angela Yu](https://www.udemy.com/course/100-days-of-code/).

---

## **Conclusion**
This analysis demonstrates the potential of Google Trends data in economic and financial forecasting. Future work could explore:
- Applying machine learning for predictive analysis.
- Expanding the dataset to include global trends.
- Investigating additional economic indicators.

