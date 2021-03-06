# COVID Data Analysis 
![Chart](as.png)
## Import the necessary module

```python
import pandas as pd
import matplotlib.pyplot as plt
```

## Reading the dataset and india map

```python
df = pd.read_csv('Covid19 India (Jan 20 - Mar 20).csv')
```

## Preview the dataset

```python
df.head()
df.tail()
```

## Shape of the dataset

```python
df.shape
```

## Info of the dataset

```python
df.info()
```

## Identify null values(if any)

```python
df.isnull().sum()
```

## Grouping data as per date

```python
df1 = df.drop(columns=['Sno', 'State/UnionTerritory']).groupby(by=['Date'], sort=False, as_index=False)
df1 = df1.sum()
df1.head()
```

## Adding new columns to existing dataset

```python
df1['Total_cases'] = df1.sum(axis=1)
df1.tail()
```

## Rate of increase of cases on each day

```python
rate_of_change = 0.0
start_date = df1.index[df1['Date'] == '04-03-2020'][0]
end_date = df1.index[df1['Date'] == '21-03-2020'][0]

for i in range(start_date, end_date):
    current_day_cases = df1.iloc[i]['Total_cases']
    next_day_cases = df1.iloc[i+1]['Total_cases']
    daily_rate = ((next_day_cases - current_day_cases) / current_day_cases)
    rate_of_change = rate_of_change + daily_rate

rate_of_change = (end_date - start_date) / rate_of_change
```

## Trend Analysis grouped by citizen's residence

```python
size = (16, 10)
fig, ax = plt.subplots(figsize = size)
ax = plt.plot(df1['Date'], df1['Total_cases'])
ax = plt.plot(df1['Date'], df1['ConfirmedIndianNational'], label = "Local Citizen")
ax = plt.plot(df1['Date'], df1['ConfirmedForeignNational'], label = "NRI")
plt.xticks(rotation = 90)
plt.xlabel('Date', fontsize = 10)
plt.ylabel('Total Cases', fontsize = 10)
```

Reference:

- <https://www.kaggle.com/aditeloo/india-s-covid-19-small-data-analysis>
