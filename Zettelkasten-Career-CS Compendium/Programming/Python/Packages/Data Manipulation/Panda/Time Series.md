---
tags:
  - FORMAT
  - Python
---
# Time Series in Pandas: Date-Time Handling, Resampling, and Frequency Conversion

Pandas provides powerful tools for working with time series data. Time series data involves timestamps or fixed time intervals and is commonly used in financial analysis, sensor data, and other real-world applications.

---

# Date-Time Handling in Pandas

## Creating Date-Time Objects

* Pandas uses `pd.to_datetime()` to convert strings, timestamps, or integers into `Datetime` objects.

### Example: Convert to DateTime

```py
import pandas as pd

 Convert string to datetime
date_series = pd.to_datetime(['2024-03-01', '2024-03-02', '2024-03-03'])
print(date_series)
```

**Output**:

```
DatetimeIndex(['2024-03-01', '2024-03-02', '2024-03-03'], dtype='datetime64[ns]', freq=None)
```

---

## Generating Date Ranges

You can create a range of dates using `pd.date_range()`.

### Example: Generate Date Range

```py
date_range = pd.date_range(start='2024-03-01', periods=7, freq='D')
print(date_range)
```

**Output**:

```
DatetimeIndex(['2024-03-01', '2024-03-02', ..., '2024-03-07'], dtype='datetime64[ns]', freq='D')
```

*   
  `freq='D'` means daily. You can also use `W` for weekly, `M` for month-end, or `H` for hourly.

---

## Extracting Date Components

You can extract useful information from date-time objects.

```py
df = pd.DataFrame({'date': pd.date_range('2024-03-01', periods=5)})
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['day'] = df['date'].dt.day
df['weekday'] = df['date'].dt.day_name()
print(df)
```

**Output**:

```
        date  year  month  day   weekday
0 2024-03-01  2024      3    1    Friday
1 2024-03-02  2024      3    2  Saturday
2 2024-03-03  2024      3    3    Sunday
3 2024-03-04  2024      3    4    Monday
4 2024-03-05  2024      3    5   Tuesday
```

---

# Resampling Time Series Data

## What is Resampling?

* Resampling involves changing the frequency of time series data.

* It can involve **up-sampling** (increasing the frequency) or **down-sampling** (reducing the frequency).

* Aggregation is often applied when down-sampling.

## Syntax:

```py
DataFrame.resample(rule).agg(func)
```

*   
  **`rule`**: Defines the frequency (`'D'`, `'W'`, `'M'`, `'Q'`, `'Y'`, `'H'`).

* **`func`**: Aggregation function like `mean`, `sum`, `min`, or `max`.

---

## Example 1: Downsampling (Reducing Frequency)

```py
date_rng = pd.date_range(start='2024-03-01', periods=12, freq='D')
df = pd.DataFrame({'date': date_rng, 'value': range(12)}).set_index('date')

 Downsample to weekly frequency
weekly_df = df.resample('W').sum()
print(weekly_df)
```

**Output**:

```
            value
date             
2024-03-03      3
2024-03-10     22
2024-03-17     33
```

*   
  Data has been aggregated to weekly sums.

---

## Example 2: Upsampling (Increasing Frequency)

```py
 Upsample from daily to hourly, using forward fill to fill missing values
hourly_df = df.resample('H').ffill()
print(hourly_df.head(10))
```

**Output**:

```
                     value
date                      
2024-03-01 00:00:00      0
2024-03-01 01:00:00      0
2024-03-01 02:00:00      0
...
2024-03-02 00:00:00      1
```

*   
  Forward fill (`ffill()`) fills the missing values using the previous value.

---

# Frequency Conversion

## What is Frequency Conversion?

* Frequency conversion refers to changing the frequency of a time series using `asfreq()`.

* Unlike `resample()`, it does not aggregate data but simply changes the frequency.

## Example: Frequency Conversion

```py
 Convert to a daily frequency, filling missing values with NaN
converted_df = df.asfreq('D')
print(converted_df)
```

*   
  **`asfreq()`** is helpful for generating a continuous time series without aggregation.

---

# Common Frequency Aliases in Pandas

| Alias | Description | Example |
| ----- | ----- | ----- |
| `D` | Calendar day | Daily data |
| `B` | Business day | Weekdays only |
| `W` | Weekly | Every Sunday |
| `M` | Month-end | Last day of month |
| `Q` | Quarter-end | Every 3 months |
| `Y` | Year-end | December 31st |
| `H` | Hourly | Every hour |
| `T` | Minute | Every minute |
| `S` | Second | Every second |

---

# Conclusion

* **Date-Time Handling**: Convert data using `pd.to_datetime()` and extract useful components with `.dt`.

* **Resampling**: Change data frequency using `resample()`. Downsample using aggregation functions or upsample using methods like `ffill()` or `bfill()`.

* **Frequency Conversion**: Use `asfreq()` for simple frequency changes without aggregation.

Pandas makes working with time series data intuitive and efficient, whether you’re analyzing financial data, monitoring sensor readings, or managing time-based records.

