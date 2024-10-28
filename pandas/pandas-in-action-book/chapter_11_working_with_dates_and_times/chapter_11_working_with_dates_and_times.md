---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.1
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

## 11.1 Introducing the Timestamp object


### 11.1.1 How Python works with datetimes

```python
import datetime as dt
import pandas as pd
```

```python
# The two lines below are equivalent
birthday = dt.date(1991, 4, 12)
birthday = dt.date(year = 1991, month = 4, day = 12)
birthday
```

```python
birthday.year
```

```python
birthday.month
```

```python
birthday.day
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# birthday.month = 10
```

```python
# The two lines below are equivalent
alarm_clock = dt.time(6, 43, 25)
alarm_clock = dt.time(hour = 6, minute = 43, second = 25)
alarm_clock
```

```python
dt.time()
```

```python
dt.time(hour = 9, second = 42)
```

```python
dt.time(hour = 19, minute = 43, second = 22)
```

```python
alarm_clock.hour
```

```python
alarm_clock.minute
```

```python
alarm_clock.second
```

```python
# The two lines below are equivalent
moon_landing = dt.datetime(1969, 7, 20, 22, 56, 20)
moon_landing = dt.datetime(
    year = 1969,
    month = 7,
    day = 20,
    hour = 22,
    minute = 56,
    second = 20
)
moon_landing
```

```python
dt.datetime(2020, 1, 1)
```

```python
dt.timedelta(
    weeks = 8,
    days = 6,
    hours = 3, 
    minutes = 58,
    seconds = 12
)
```

### 11.1.2 How `pandas` works with datetimes

```python
# The two lines below are equivalent
pd.Timestamp(1991, 4, 12)
pd.Timestamp(year = 1991, month = 4, day = 12)
```

```python
(pd.Timestamp(year = 1991, month = 4, day = 12) 
 == dt.date(year = 1991, month = 4, day = 12))
```

```python
(pd.Timestamp(year = 1991, month = 4, day = 12, minute = 2)
 == dt.datetime(year = 1991, month = 4, day = 12, minute = 2))
```

```python
(pd.Timestamp(year = 1991, month = 4, day = 12, minute = 2) 
 == dt.datetime(year = 1991, month = 4, day = 12, minute = 1))
```

```python
pd.Timestamp("2015-03-31")
```

```python
pd.Timestamp("2015/03/31")
```

```python
pd.Timestamp("03/31/2015")
```

```python
pd.Timestamp("2021-03-08 08:35:15")
```

```python
pd.Timestamp("2021-03-08 6:13:29 PM")
```

```python
pd.Timestamp(dt.datetime(2000, 2, 3, 21, 35, 22))
```

```python
my_time = pd.Timestamp(dt.datetime(2000, 2, 3, 21, 35, 22))
print(my_time.year) 
print(my_time.month)
print(my_time.day)
print(my_time.hour) 
print(my_time.minute)
print(my_time.second)
```

## 11.2 Storing Multiple Timestamps in a DatetimeIndex

```python
pd.Series([1, 2, 3]).index
```

```python
pd.Series([1, 2, 3], index = ["A", "B", "C"]).index
```

```python
timestamps = [
    pd.Timestamp("2020-01-01"),
    pd.Timestamp("2020-02-01"),
    pd.Timestamp("2020-03-01"),
]

pd.Series([1, 2, 3], index = timestamps).index
```

```python
datetimes = [
    dt.datetime(2020, 1, 1),
    dt.datetime(2020, 2, 1),
    dt.datetime(2020, 3, 1),
]

pd.Series([1, 2, 3], index = datetimes).index
```

```python
string_dates = ["2018/01/02", "2016/04/12", "2009/09/07"]
pd.DatetimeIndex(string_dates)
```

```python
mixed_dates = [
    dt.date(2018, 1, 2),
    "2016/04/12",
    pd.Timestamp(2009, 9, 7)
]

dt_index = pd.DatetimeIndex(mixed_dates)
dt_index
```

```python
s = pd.Series(data = [100, 200, 300], index = dt_index)
s
```

```python
s.sort_index()
```

```python
morning = pd.Timestamp("2020-01-01 11:23:22 AM")
evening = pd.Timestamp("2020-01-01 11:23:22 PM")

morning < evening
```

## 11.3	Converting Column or Index Values to Datetimes

```python
disney = pd.read_csv("disney.csv")
disney.head()
```

```python
disney.dtypes
```

```python
disney = pd.read_csv("disney.csv", parse_dates = ["Date"])
```

```python
string_dates = ["2015-01-01", "2016-02-02", "2017-03-03"]
dt_index = pd.to_datetime(string_dates)
dt_index
```

```python
pd.to_datetime(disney["Date"]).head()
```

```python
disney["Date"] = pd.to_datetime(disney["Date"])
```

```python
disney.dtypes
```

## 11.4 Using the DatetimeProperties Object

```python
disney["Date"].dt
```

```python
disney["Date"].head(3)
```

```python
disney["Date"].dt.day.head(3)
```

```python
disney["Date"].dt.month.head(3)
```

```python
disney["Date"].dt.year.head(3)
```

```python
disney["Date"].dt.dayofweek.head()
```

```python
disney["Date"].dt.day_name().head()
```

```python
disney["Day of Week"] = disney["Date"].dt.day_name()
```

```python
group = disney.groupby("Day of Week")
```

```python
group.mean()
```

```python
disney["Date"].dt.month_name().head()
```

```python
disney["Date"].dt.is_quarter_start.tail()
```

```python
disney[disney["Date"].dt.is_quarter_start].head()
```

```python
disney[disney["Date"].dt.is_quarter_end].head()
```

```python
disney[disney["Date"].dt.is_month_start].head()
```

```python
disney[disney["Date"].dt.is_month_end].head()
```

```python
disney[disney["Date"].dt.is_year_start].head()
```

```python
disney[disney["Date"].dt.is_year_end].head()
```

## 11.5 Adding and Subtracting Durations of Time

```python
pd.DateOffset(years = 3, months = 4, days = 5)
```

```python
disney["Date"].head()
```

```python
(disney["Date"] + pd.DateOffset(days = 5)).head()
```

```python
(disney["Date"] - pd.DateOffset(days = 3)).head()
```

```python
(disney["Date"] + pd.DateOffset(days = 10, hours = 6)).head()
```

```python
(
    disney["Date"]
    - pd.DateOffset(
        years = 1, months = 3, days = 10, hours = 6, minutes = 3
    )
).head()
```

## 11.6 Date Offsets

```python
disney["Date"].tail()
```

```python
(disney["Date"] + pd.offsets.MonthEnd()).tail()
```

```python
(disney["Date"] - pd.offsets.MonthEnd()).tail()
```

```python
(disney["Date"] + pd.offsets.MonthBegin()).tail()
```

```python
(disney["Date"] - pd.offsets.MonthBegin()).tail()
```

```python
may_dates = ["2020-05-28", "2020-05-29", "2020-05-30"]
end_of_may = pd.Series(pd.to_datetime(may_dates))
end_of_may
```

```python
end_of_may + pd.offsets.MonthEnd()
```

```python
end_of_may + pd.offsets.BMonthEnd()
```

## 11.7 The `timedelta` Object

```python
duration = pd.Timedelta(
    days = 8,
    hours = 7,
    minutes = 6,
    seconds = 5
)

duration
```

```python
pd.to_timedelta("3 hours, 5 minutes, 12 seconds")
```

```python
pd.to_timedelta(5, unit = "hour")
```

```python
pd.to_timedelta([5, 10, 15], unit = "day")
```

```python
pd.Timestamp("1999-02-05") - pd.Timestamp("1998-05-24")
```

```python
deliveries = pd.read_csv("deliveries.csv")
deliveries.head()
```

```python
deliveries["order_date"] = pd.to_datetime(
    deliveries["order_date"]
)

deliveries["delivery_date"] = pd.to_datetime(
    deliveries["delivery_date"]
)
```

```python
for column in ["order_date", "delivery_date"]:
    deliveries[column] = pd.to_datetime(deliveries[column])
```

```python
deliveries.head()
```

```python
(deliveries["delivery_date"] - deliveries["order_date"]).head()
```

```python
deliveries["duration"] = (
    deliveries["delivery_date"] - deliveries["order_date"]
)
deliveries.head()
```

```python
deliveries.dtypes
```

```python
(deliveries["delivery_date"] - deliveries["duration"]).head()
```

```python
(deliveries["delivery_date"] + deliveries["duration"]).head()
```

```python
deliveries.sort_values("duration")
```

```python
deliveries["duration"].max()
```

```python
deliveries["duration"].min()
```

```python
deliveries["duration"].mean()
```

```python
# The two lines below are equivalent
(deliveries["duration"] > pd.Timedelta(days = 365)).head() 
(deliveries["duration"] > "365 days").head()
```

```python
deliveries[deliveries["duration"] > "365 days"].head()
```

```python
long_time = (
    deliveries["duration"] > "2000 days, 8 hours, 4 minutes"
)

deliveries[long_time].head()
```

## 11.8 Coding Challenge


### 11.8.1 Problems

```python
citi_bike = pd.read_csv("citibike.csv")
citi_bike.head()
```

```python
citi_bike.info()
```

### 11.8.2. Solutions

```python
for column in ["start_time", "stop_time"]:
    citi_bike[column] = pd.to_datetime(citi_bike[column])
```

```python
citi_bike.info()
```

```python
citi_bike["start_time"].dt.day_name().head()
```

```python
citi_bike["start_time"].dt.day_name().value_counts()
```

```python
citi_bike["start_time"].dt.dayofweek.head()
```

```python
days_away_from_monday = citi_bike["start_time"].dt.dayofweek
```

```python
citi_bike["start_time"] - pd.to_timedelta(
    days_away_from_monday, unit = "day"
)
```

```python
dates_rounded_to_monday = citi_bike[
    "start_time"
] - pd.to_timedelta(days_away_from_monday, unit = "day")
```

```python
dates_rounded_to_monday.value_counts().head()
```

```python
dates_rounded_to_monday.dt.date.head()
```

```python
dates_rounded_to_monday.dt.date.value_counts()
```

```python
citi_bike["duration"] = (
    citi_bike["stop_time"] - citi_bike["start_time"]
)

citi_bike.head()
```

```python
citi_bike["duration"].mean()
```

```python
citi_bike["duration"].sort_values(ascending = False).head()
```

```python
citi_bike.nlargest(n = 5, columns = "duration")
```

## 11.9 Summary

```python

```
