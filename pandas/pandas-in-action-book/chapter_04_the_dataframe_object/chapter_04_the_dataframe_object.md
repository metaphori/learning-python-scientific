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

## 4.1 Overview of a DataFrame

```python
import pandas as pd
import numpy as np
```

### 4.1.1 Creating DataFrame from Dictionary

```python
city_data = {
    "City": ["New York City", "Paris", "Barcelona", "Rome"],
    "Country": ["United States", "France", "Spain", "Italy"],
    "Population": pd.Series([8600000, 2141000, 5515000, 2873000])
}

cities = pd.DataFrame(city_data)
cities
```

```python
# The two lines below are equivalent
cities.transpose()
cities.T
```

### 4.1.2 Creating a DataFrame from a NumPy ndarray

```python
random_data = np.random.randint(1, 101, [3, 5])
random_data
```

```python
pd.DataFrame(data = random_data)
```

```python
row_labels = ["Morning", "Afternoon", "Evening"]
temperatures = pd.DataFrame(
    data = random_data, index = row_labels
)
temperatures
```

```python
row_labels = ["Morning", "Afternoon", "Evening"]
column_labels = (
    "Monday",
    "Tuesday",
    "Wednesday",
    "Thursday",
    "Friday"
)

pd.DataFrame(
    data = random_data,
    index = row_labels,
    columns = column_labels,
)
```

```python
row_labels = ["Morning", "Afternoon", "Morning"]
column_lables = [
    "Monday",
    "Tuesday",
    "Wednesday",
    "Tuesday",
    "Friday"
]

pd.DataFrame(
    data = random_data,
    index = row_labels,
    columns = column_labels,
)

```

## 4.2 Similarities between Series and DataFrames


### 4.2.1 Importing a DataFrame with the read_csv Function

```python
pd.read_csv("nba.csv")
```

```python
pd.read_csv("nba.csv", parse_dates = ["Birthday"])
```

```python
nba = pd.read_csv("nba.csv", parse_dates = ["Birthday"])
```

### 4.2.2 Shared and Exclusive Attributes between Series and DataFrames

```python
pd.Series([1, 2, 3]).dtype
```

```python
nba.dtypes
```

```python
nba.dtypes.value_counts()
```

```python
nba.index
```

```python
nba.columns
```

```python
nba.ndim
```

```python
nba.shape
```

```python
nba.size
```

```python
nba.count()
```

```python
nba.count().sum()
```

```python
data = {
    "A": [1, np.nan],
    "B": [2, 3]
}

df = pd.DataFrame(data)
df
```

```python
df.size
```

```python
df.count()
```

```python
df.count().sum()
```

### 4.2.3 Shared Methods between Series and DataFrames

```python
nba.head(2)
```

```python
nba.tail(n = 3)
```

```python
nba.tail()
```

```python
nba.sample(3)
```

```python
nba.nunique()
```

```python
nba.max()
```

```python
nba.min()
```

```python
nba.nlargest(n = 4, columns = "Salary")
```

```python
nba.nsmallest(n = 3, columns = ["Birthday"])
```

```python
nba.sum()
```

```python
nba.sum(numeric_only = True)
```

```python
nba.mean(numeric_only = True)
```

```python
nba.median(numeric_only = True)
```

```python
nba.mode(numeric_only = True)
```

```python
nba.std(numeric_only = True)
```

## 4.3 Sorting a DataFrame


### 4.3.1 Sorting by Single Column

```python
# The two lines below are equivalent
nba.sort_values("Name")
nba.sort_values(by = "Name")
```

```python
nba.sort_values("Name", ascending = False).head()
```

```python
nba.sort_values("Birthday", ascending = False).head()
```

### 4.3.2 Sorting by Multiple Columns

```python
nba.sort_values(by = ["Team", "Name"])
```

```python
nba.sort_values(["Team", "Name"], ascending = False)
```

```python
nba.sort_values(
    by = ["Team", "Salary"], ascending = [True, False]
)
```

```python
nba = nba.sort_values(
    by = ["Team", "Salary"],
    ascending = [True, False]
)
```

## 4.4 Sorting by Index

```python
nba.head()
```

### 4.4.1 Sorting by Row Index

```python
# The two lines below are equivalent
nba.sort_index().head()
nba.sort_index(ascending = True).head()
```

```python
nba.sort_index(ascending = False).head()
```

```python
nba = nba.sort_index()
```

### 4.4.2 Sorting by Column Index

```python
# The two lines below are equivalent
nba.sort_index(axis = "columns").head()
nba.sort_index(axis = 1).head()
```

```python
nba.sort_index(axis = "columns", ascending = False).head()
```

## 4.5 Setting a New Index

```python
# The two lines below are equivalent
nba.set_index(keys = "Name")
nba.set_index("Name")
```

```python
nba = nba.set_index(keys = "Name")
```

```python
nba = pd.read_csv(
    "nba.csv", parse_dates = ["Birthday"], index_col = "Name"
)
```

## 4.6 Selecting Columns and Rows from a DataFrame


### 4.6.1 Selecting a Single Column from a DataFrame

```python
nba.Salary
```

```python
nba["Position"]
```

### 4.6.2 Selecting Multiple Columns from a DataFrame

```python
nba[["Salary", "Birthday"]].head()
```

```python
nba[["Birthday", "Salary"]].head()
```

```python
nba.select_dtypes(include = "object")
```

```python
nba.select_dtypes(exclude = ["object", "int"])
```

## 4.7 Selecting Rows from a DataFrame


### 4.7.1 Extracting Rows by Index Label

```python
nba.loc["LeBron James"]
```

```python
nba.loc[["Kawhi Leonard", "Paul George"]]
```

```python
nba.loc[["Paul George", "Kawhi Leonard"]]
```

```python
nba.sort_index().loc["Otto Porter":"Patrick Beverley"]
```

```python
players = ["Otto Porter", "PJ Dozier", "PJ Washington"]
players[0:2]
```

```python
nba.sort_index().loc["Zach Collins":]
```

```python
nba.sort_index().loc[:"Al Horford"]
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# nba.loc["Bugs Bunny"]
```

### 4.7.2 Extracting Rows by Index Position

```python
nba.iloc[300]
```

```python
nba.iloc[[100, 200, 300, 400]]
```

```python
nba.iloc[400:404]
```

```python
nba.iloc[:2]
```

```python
nba.iloc[447:]
```

```python
nba.iloc[-10:-6]
```

```python
nba.iloc[0:10:2]
```

### 4.7.3 Extracting Values from Specific Columns

```python
nba.loc["Giannis Antetokounmpo", "Team"]
```

```python
nba.loc["James Harden", ["Position", "Birthday"]]
```

```python
nba.loc[
    ["Russell Westbrook", "Anthony Davis"],
    ["Team", "Salary"]
]
```

```python
nba.loc["Joel Embiid", "Position":"Salary"]
```

```python
nba.loc["Joel Embiid", "Salary":"Position"]
```

```python
nba.iloc[57, 3]
```

```python
nba.iloc[100:104, :3]
```

```python
nba.at["Austin Rivers", "Birthday"]
```

```python
nba.iat[263, 1]
```

```python
%%timeit
nba.at["Austin Rivers", "Birthday"]
```

```python
%%timeit
nba.loc["Austin Rivers", "Birthday"]
```

```python
%%timeit
nba.iat[263, 1]
```

```python
%%timeit
nba.iloc[263, 1]
```

## 4.8 Extracting Values from Series

```python
nba["Salary"].loc["Damian Lillard"]
```

```python
nba["Salary"].at["Damian Lillard"]
```

```python
nba["Salary"].iloc[234]
```

```python
nba["Salary"].iat[234]
```

## 4.9 Renaming Columns or Rows

```python
nba.columns
```

```python
nba.columns = ["Team", "Position", "Date of Birth", "Pay"]
nba.head(1)
```

```python
nba.rename(columns = { "Date of Birth": "Birthday" })
```

```python
nba = nba.rename(columns = { "Date of Birth": "Birthday" })
```

```python
nba.loc["Giannis Antetokounmpo"]
```

```python
nba = nba.rename(
    index = { "Giannis Antetokounmpo": "Greek Freak" }    
)
```

```python
nba.loc["Greek Freak"]
```

## 4.10 Resetting an Index

```python
nba.set_index("Team").head()
```

```python
nba.reset_index().head()
```

```python
nba.reset_index().set_index("Team").head()
```

```python
nba = nba.reset_index().set_index("Team")
```

## 4.11 Coding Challenge


### 4.11.1 Problems


### 4.11.2 Solutions

```python
nfl = pd.read_csv("nfl.csv", parse_dates = ["Birthday"])
nfl
```

```python
nfl = nfl.set_index("Name")
```

```python
nfl = pd.read_csv("nfl.csv", index_col = "Name", parse_dates = ["Birthday"])
```

```python
nfl.head()
```

```python
# The two lines below are equivalent
nfl.Team.value_counts().head()
nfl["Team"].value_counts().head()
```

```python
nfl.sort_values("Salary", ascending = False).head()
```

```python
nfl.sort_values(
    by = ["Team", "Salary"],
    ascending = [True, False]
)
```

```python
nfl = nfl.reset_index().set_index(keys = "Team")
nfl.head(3)
```

```python
nfl.loc["New York Jets"].head()
```

```python
nfl.loc["New York Jets"].sort_values("Birthday").head(1)
```

## 4.12 Summary

```python

```
