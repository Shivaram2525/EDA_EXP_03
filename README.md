# EXP 3 - Delhi Air Quality Analysis

```
Name         : Shivaram M.
Register No. : 212223040195
```
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

```
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
df=pd.read_csv('/Users/home/Desktop/Workspace/College/Semester_5/EDA/Experiments/EXP_3/Dataset/delhi_dataset_3.csv')
df.head()
df.info()
df.describe()
df['period.datetimeFrom.utc']=pd.to_datetime(df['period.datetimeFrom.utc'],errors='coerce')
df['value']=pd.to_numeric(df['value'],errors='coerce')
df.dropna(subset=['period.datetimeFrom.utc','value'],inplace=True)
df['date']=df['period.datetimeFrom.utc'].dt.date
df['month']=df['period.datetimeFrom.utc'].dt.month_name()
df['year']=df['period.datetimeFrom.utc'].dt.year
df['hour']=df['period.datetimeFrom.utc'].dt.hour
plt.figure(figsize=(10, 6))
sns.boxplot(x='month', y='value', data=df,
            order=['January','February','March','April','May','June',
                   'July','August','September','October','November','December'],
            palette='coolwarm')
plt.xticks(rotation=45)
plt.title('Monthly Distribution of PM2.5 Concentration')
plt.xlabel('Month')
plt.ylabel('PM2.5 (µg/m³)')
plt.tight_layout()
plt.show()
monthly_avg = df.groupby('month')['value'].mean().reindex(
    ['January','February','March','April','May','June',
     'July','August','September','October','November','December']
)
plt.figure(figsize=(10, 5))
sns.lineplot(x=monthly_avg.index, y=monthly_avg.values, marker='o', color='crimson')
plt.xticks(rotation=45)
plt.title('Monthly Average PM2.5 Levels')
plt.xlabel('Month')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.tight_layout()
plt.show()
print("\nMonthly Average PM2.5 (µg/m³):")
print(monthly_avg.round(2))
daily_avg = df.groupby('date')['value'].mean()
total_days = len(daily_avg)
exceed_days = (daily_avg > 25).sum()
exceed_pct = (exceed_days / total_days) * 100

print(f"\nTotal days recorded: {total_days}")
print(f"Days exceeding WHO PM2.5 limit (25 µg/m³): {exceed_days}")
print(f"Percentage of exceedance days: {exceed_pct:.2f}%")

hourly_avg = df.groupby('hour')['value'].mean()

plt.figure(figsize=(10,5))
sns.lineplot(x=hourly_avg.index, y=hourly_avg.values, marker='o', color='darkorange')
plt.xticks(range(0, 24))
plt.title('Average PM2.5 by Hour of Day')
plt.xlabel('Hour (0–23)')
plt.ylabel('Average PM2.5 (µg/m³)')
plt.grid(True)
plt.tight_layout()
plt.show()

print("\nAverage PM2.5 by Hour of Day:")
print(hourly_avg.round(2))
top5 = daily_avg.sort_values(ascending=False).head(5)
print("\nTop 5 Worst-Polluted Days (Highest Daily Average PM2.5):")
print(top5.round(2))


```

## Output
<img width="1680" height="1050" alt="1" src="https://github.com/user-attachments/assets/a8859f62-eb39-424d-bdb2-74064a2349f5" />
<img width="1680" height="1050" alt="2" src="https://github.com/user-attachments/assets/7d875006-c0c0-4bb3-bafd-5523a88b3699" />
<img width="1680" height="1050" alt="3" src="https://github.com/user-attachments/assets/a6d94e10-0488-4d59-bcb2-cd05a3c1f670" />
<img width="1680" height="1050" alt="4" src="https://github.com/user-attachments/assets/f10d3c7e-4638-463b-af6f-fad831bcf1b6" />
<img width="1680" height="1050" alt="5" src="https://github.com/user-attachments/assets/c4dc2c6a-f029-4b0b-a7cf-1d8740e896b8" />
<img width="1680" height="1050" alt="6" src="https://github.com/user-attachments/assets/306e0167-657d-454a-925d-839657008f8d" />
<img width="1680" height="1050" alt="7" src="https://github.com/user-attachments/assets/eed43f21-0a8a-4963-9963-1770f613019e" />

## Interpretation

1)PM2.5 and NO₂ show a strong positive correlation, suggesting that both pollutants increase together, likely due to vehicle and industrial emissions.

## write other insights

## Interpretation:
### 1. The monthly analysis shows PM2.5 levels peak during winter (Nov–Jan)
###  and drop during the monsoon months (Jun–Aug), consistent with reduced dispersion
###  and increased emissions in cold, stagnant air.
### 2. The diurnal pattern indicates higher PM2.5 concentrations at night
###  and early morning, likely due to temperature inversion and traffic peaks.
### 3. Every day exceeding the WHO daily limit (25 µg/m³) reflects severe, chronic
###  air pollution conditions—posing major respiratory and cardiovascular risks.

## Result

The dataset was successfully loaded and processed to extract pollutant-wise and station-wise air quality data for Delhi.


