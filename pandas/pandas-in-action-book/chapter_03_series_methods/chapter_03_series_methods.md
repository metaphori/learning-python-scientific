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

## 3.1 Importing a Data Set with the read_csv Function

```python
import pandas as pd
```

```python
# The two lines below are equivalent
pd.read_csv(filepath_or_buffer = "pokemon.csv")
pd.read_csv("pokemon.csv")
```

```python
pd.read_csv("pokemon.csv", index_col = "Pokemon")
```

```python
pd.read_csv("pokemon.csv", index_col = "Pokemon", squeeze = True)
```

```python
pokemon = pd.read_csv(
    "pokemon.csv", index_col = "Pokemon", squeeze = True
)
```

```python
pd.read_csv("google_stocks.csv").head()
```

```python
pd.read_csv("google_stocks.csv", parse_dates = ["Date"]).head()
```

```python
pd.read_csv(
    "google_stocks.csv",
    parse_dates = ["Date"],
    index_col = "Date",
    squeeze = True
).head()
```

```python
google = pd.read_csv(
    "google_stocks.csv",
    parse_dates = ["Date"],
    index_col = "Date",
    squeeze = True
)
```

```python
pd.read_csv("revolutionary_war.csv").tail() 
```

```python
pd.read_csv(
    "revolutionary_war.csv",
    index_col = "Start Date",
    parse_dates = ["Start Date"],
).tail()
```

```python
pd.read_csv(
    "revolutionary_war.csv",
    index_col = "Start Date",
    parse_dates = ["Start Date"],
    usecols = ["State", "Start Date"],
    squeeze = True,
).tail()
```

```python
battles = pd.read_csv(
    "revolutionary_war.csv",
    index_col = "Start Date",
    parse_dates = ["Start Date"],
    usecols = ["State", "Start Date"],
    squeeze = True,
)
```

## 3.2 Sorting a Series


### 3.2.1 Sorting by Values with the sort_values Method

```python
google.sort_values()
```

```python
pokemon.sort_values()
```

```python
pd.Series(data = ["Adam", "adam", "Ben"]).sort_values()
```

```python
google.sort_values(ascending = False).head()
```

```python
pokemon.sort_values(ascending = False).head()
```

```python
# The two lines below are equivalent
battles.sort_values()
battles.sort_values(na_position = "last")
```

```python
battles.sort_values(na_position = "first")
```

```python
battles.dropna().sort_values()
```

### 3.2.2 Sorting by Index with the sort_index Method

```python
# The two lines below are equivalent
pokemon.sort_index()
pokemon.sort_index(ascending = True)
```

```python
battles.sort_index()
```

```python
battles.sort_index(na_position = "first").head()
```

```python
battles.sort_index(ascending = False).head()
```

### 3.2.3 Retrieving the Smallest and Largest Values with the nsmallest and nlargest Methods

```python
google.sort_values(ascending = False).head()
```

```python
# The two lines below are equivalent
google.nlargest(n = 5)
google.nlargest()
```

```python
# The two lines below are equivalent
google.nsmallest(n = 5)
google.nsmallest(5)
```

## 3.3 Overwriting a Series with the inplace Parameter

```python
battles.head(3)
```

```python
battles.sort_values().head(3)
```

```python
battles.head(3)
```

```python
battles.head(3)
```

```python
battles.sort_values(inplace = True)
```

```python
battles.head(3)
```

## 3.4 Counting Values with the value_counts Method

```python
pokemon.head()
```

```python
pokemon.value_counts()
```

```python
len(pokemon.value_counts())
```

```python
pokemon.nunique()
```

```python
pokemon.value_counts(ascending = True)
```

```python
pokemon.value_counts(normalize = True).head()
```

```python
pokemon.value_counts(normalize = True).head() * 100
```

```python
(pokemon.value_counts(normalize = True) * 100).round(2)
```

```python
google.value_counts().head()
```

```python
google.max()
```

```python
google.min()
```

```python
buckets = [0, 200, 400, 600, 800, 1000, 1200, 1400]
google.value_counts(bins = buckets)
```

```python
google.value_counts(bins = buckets).sort_index()
```

```python
google.value_counts(bins = buckets, sort = False)
```

```python
google.value_counts(bins = 6, sort = False)
```

```python
battles.head()
```

```python
battles.value_counts().head()
```

```python
battles.value_counts(dropna = False).head()
```

```python
battles.index
```

```python
battles.index.value_counts()
```

## 3.5 Invoking a Function on Every Series Value with the apply Method

```python
funcs = [len, max, min]
```

```python
funcs = [len, max, min]

for current_func in funcs:
    print(current_func(google))
```

```python
round(99.2)
```

```python
round(99.49)
```

```python
round(99.5)
```

```python
# The two lines below are equivalent
google.apply(func = round)
google.apply(round)
```

```python
def single_or_multi(pokemon_type):
    if "/" in pokemon_type:
        return "Multi"
    
    return "Single"
```

```python
pokemon.head(4)
```

```python
pokemon.apply(single_or_multi)
```

```python
pokemon.apply(single_or_multi).value_counts()
```

## 3.6 Coding Challenge


### 3.6.1 Problems


### 3.6.2 Solutions

```python
import datetime as dt
today = dt.datetime(2020, 12, 26)
today.strftime("%A")
```

```python
pd.read_csv("revolutionary_war.csv").head()
```

```python
days_of_war = pd.read_csv(
    "revolutionary_war.csv",
    usecols = ["Start Date"],
    parse_dates = ["Start Date"],
    squeeze = True,
)

days_of_war.head()
```

```python
def day_of_week(date):
    return date.strftime("%A")
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# days_of_war.apply(day_of_week)
```

```python
days_of_war.dropna().apply(day_of_week)
```

```python
days_of_war.dropna().apply(day_of_week).value_counts()
```

## 3.7 Summary

```python

```
