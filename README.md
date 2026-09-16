# EXP 3 - Delhi Air Quality Analysis

## Aim


To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.


## Procedure / Algorithm

1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.


## Program

### Name : SUDARSHANA S

### Reg No: 212223050054

```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv("/content/delhi_pm25_aqi.csv")
df.head()

print("First 5 rows:")
print(df.head())

print("\nData types and non-null counts:")
print(df.info())

print("\nNull values per column:")
print(df.isnull().sum())

df.shape
```

```python
df['datetime'] = pd.to_datetime(
df['period.datetimeFrom.utc'],
errors='coerce'
)

df['value'] = pd.to_numeric(df['value'], errors='coerce')
df = df.dropna(subset=['datetime', 'value'])
```

```python
df['date'] = df['datetime'].dt.date
df['month'] = df['datetime'].dt.month_name()
df['hour'] = df['datetime'].dt.hour
```
```python
plt.figure(figsize=(12,6))
month_order = [
'January','February','March','April','May','June',
'July','August','September','October','November','December'
]
sns.boxplot(
x='month',
y='value',
data=df,
order=month_order
)
plt.xticks(rotation=45)
plt.title("Monthly Distribution of PM2.5 – Delhi")
plt.xlabel("Month")
plt.ylabel("PM2.5 (µg/m³)")
plt.tight_layout()
plt.show()
```

```python
monthly_avg = df.groupby('month')['value'].mean().reindex(month_order)
plt.figure(figsize=(10,5))
monthly_avg.plot(kind='bar')
plt.title("Monthly Average PM2.5 – Delhi")
plt.xlabel("Month")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.tight_layout()
plt.show()
```

```python
WHO_LIMIT = 25
# Daily average PM2.5
daily_avg = df.groupby('date')['value'].mean()
total_days = daily_avg.shape[0]
exceed_days = (daily_avg > WHO_LIMIT).sum()
percentage_exceed = (exceed_days / total_days) * 100
print(f"Total days: {total_days}")
print(f"Days exceeding WHO limit: {exceed_days}")
print(f"Percentage of unsafe days: {percentage_exceed:.2f}%")
```

```python
hourly_avg = df.groupby('hour')['value'].mean().reset_index()
plt.figure(figsize=(10,5))
sns.lineplot(x='hour', y='value', data=hourly_avg, marker='o')
plt.title("Average PM2.5 by Hour of Day – Delhi")
plt.xlabel("Hour of Day")
plt.ylabel("PM2.5 (µg/m³)")
plt.xticks(range(0,24))
plt.tight_layout()
plt.show()
```

```python
top5_days = daily_avg.sort_values(ascending=False).head(5)
print("=== TOP 5 WORST-POLLUTED DAYS ===")
print(top5_days, "\n")
```
## Output

### Load the dataset

<img width="742" height="227" alt="image" src="https://github.com/user-attachments/assets/6e3b6e6b-e2ce-4bd2-a70d-3db9279524e9" />

### summary (head, data types, null counts)

<img width="790" height="572" alt="image" src="https://github.com/user-attachments/assets/1fccd579-6d04-4ba9-81e5-5c7ca5f68b92" />

### Add date, month, hour columns

<img width="1117" height="464" alt="image" src="https://github.com/user-attachments/assets/99f5bf30-6e02-44a1-ae1e-8d02982cf9c6" />

### Plot monthly boxplots of PM2.5

<img width="1477" height="664" alt="image" src="https://github.com/user-attachments/assets/ea8d1593-1391-4d99-b6dd-2e90cf3c89ec" />

### monthly average PM2.5

<img width="1172" height="565" alt="image" src="https://github.com/user-attachments/assets/16ba1d32-b48b-4b2c-9add-b733a9fedd7c" />

### days exceed WHO PM2.5 limit (25 µg/m³) and percentage

<img width="729" height="65" alt="image" src="https://github.com/user-attachments/assets/d52b6de8-2144-49d9-a589-4a6ab2de6d63" />

### average PM2.5 vs hour-of-day
<img width="1287" height="547" alt="image" src="https://github.com/user-attachments/assets/bd2fcbd3-cc2a-4a8c-b893-93e3b022a1a9" />

### Top 5 worst-polluted days
<img width="762" height="172" alt="image" src="https://github.com/user-attachments/assets/6aaeaab8-926c-4363-b059-af5fe57f9131" />


## Interpretation 

1) PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

2) PM2.5 levels drop significantly during monsoon months, peak in winter due to stagnant air and emissions, and show higher concentrations during traffic hours, highlighting the impact of weather and human activity on pollution.

## Result

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.

