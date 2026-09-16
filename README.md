EXP 3 - Delhi Air Quality Analysis
Aim
To compare air quality parameters in Delhi across different stations and analyze the relationship between pollutants (e.g., PM2.5 and NO₂) using scatter plots and correlation analysis.

Procedure / Algorithm
1)Load the dataset using pandas.

2)Preprocess the data:

3)Convert the date column (period.datetimeFrom.utc) to datetime format.

4)Drop missing or invalid values.

5)Pivot the dataset so each pollutant (parameter) becomes a separate column.

6)Plot scatter plot between PM2.5 and NO₂ to study their relationship.

7)Plot correlation heatmap between all pollutants to identify relationships.

8)Interpret the results — identify which pollutants are correlated and which stations are most polluted.

Program
Name : SUDARSHANA S
Reg No: 212223050054
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
df['datetime'] = pd.to_datetime(
df['period.datetimeFrom.utc'],
errors='coerce'
)

df['value'] = pd.to_numeric(df['value'], errors='coerce')
df = df.dropna(subset=['datetime', 'value'])
df['date'] = df['datetime'].dt.date
df['month'] = df['datetime'].dt.month_name()
df['hour'] = df['datetime'].dt.hour
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
monthly_avg = df.groupby('month')['value'].mean().reindex(month_order)
plt.figure(figsize=(10,5))
monthly_avg.plot(kind='bar')
plt.title("Monthly Average PM2.5 – Delhi")
plt.xlabel("Month")
plt.ylabel("Average PM2.5 (µg/m³)")
plt.tight_layout()
plt.show()
WHO_LIMIT = 25
# Daily average PM2.5
daily_avg = df.groupby('date')['value'].mean()
total_days = daily_avg.shape[0]
exceed_days = (daily_avg > WHO_LIMIT).sum()
percentage_exceed = (exceed_days / total_days) * 100
print(f"Total days: {total_days}")
print(f"Days exceeding WHO limit: {exceed_days}")
print(f"Percentage of unsafe days: {percentage_exceed:.2f}%")
hourly_avg = df.groupby('hour')['value'].mean().reset_index()
plt.figure(figsize=(10,5))
sns.lineplot(x='hour', y='value', data=hourly_avg, marker='o')
plt.title("Average PM2.5 by Hour of Day – Delhi")
plt.xlabel("Hour of Day")
plt.ylabel("PM2.5 (µg/m³)")
plt.xticks(range(0,24))
plt.tight_layout()
plt.show()
top5_days = daily_avg.sort_values(ascending=False).head(5)
print("=== TOP 5 WORST-POLLUTED DAYS ===")
print(top5_days, "\n")
Output
Load the dataset
image
summary (head, data types, null counts)
image
Add date, month, hour columns
image
Plot monthly boxplots of PM2.5
image
monthly average PM2.5
image
days exceed WHO PM2.5 limit (25 µg/m³) and percentage
image
average PM2.5 vs hour-of-day
image
Top 5 worst-polluted days
image
Interpretation
PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

PM2.5 levels drop significantly during monsoon months, peak in winter due to stagnant air and emissions, and show higher concentrations during traffic hours, highlighting the impact of weather and human activity on pollution.

Result
The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


