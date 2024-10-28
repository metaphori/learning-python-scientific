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

## 7.1 The MultiIndex Object

```python
import pandas as pd
```

```python
address = ("8809 Flair Square", "Toddside", "IL", "37206")
address
```

```python
addresses = [
    ("8809 Flair Square", "Toddside", "IL", "37206"),
    ("9901 Austin Street", "Toddside", "IL", "37206"),
    ("905 Hogan Quarter", "Franklin", "IL", "37206"),
]
```

```python
# The two lines below are equivalent
pd.MultiIndex.from_tuples(addresses)
pd.MultiIndex.from_tuples(tuples = addresses)
```

```python
row_index = pd.MultiIndex.from_tuples(
    tuples = addresses,
    names = ["Street", "City", "State", "Zip"]
)

row_index
```

```python
data = [
    ["A", "B+"],
    ["C+", "C"],
    ["D-", "A"],
]

columns = ["Schools", "Cost of Living"]

area_grades = pd.DataFrame(
    data = data, index = row_index, columns = columns
)

area_grades
```

```python
area_grades.columns
```

```python
column_index = pd.MultiIndex.from_tuples([
    ("Culture", "Restaurants"),
    ("Culture", "Museums"),
    ("Services", "Police"),
    ("Services", "Schools"),
])

column_index
```

```python
data = [
    ["C-", "B+", "B-", "A"],
    ["D+", "C", "A", "C+"],
    ["A-", "A", "D+", "F"], 
]
```

```python
pd.DataFrame(
    data = data, index = row_index, columns = column_index
)
```

## 7.2 MultiIndex DataFrames

```python
neighborhoods = pd.read_csv("neighborhoods.csv")
neighborhoods.head()
```

```python
neighborhoods = pd.read_csv(
    "neighborhoods.csv",
    index_col = [0, 1, 2]
)

neighborhoods.head()
```

```python
neighborhoods = pd.read_csv(
    "neighborhoods.csv",
    index_col = [0, 1, 2],
    header = [0, 1]
)

neighborhoods.head()
```

```python
neighborhoods.info()
```

```python
neighborhoods.index
```

```python
neighborhoods.columns
```

```python
neighborhoods.index.names
```

```python
# The two lines below are equivalent
neighborhoods.index.get_level_values(1)
neighborhoods.index.get_level_values("City")
```

```python
neighborhoods.columns.names
```

```python
neighborhoods.columns.names = ["Category", "Subcategory"]
neighborhoods.columns.names
```

```python
neighborhoods.head(3)
```

```python
# The two lines below are equivalent
neighborhoods.columns.get_level_values(0)
neighborhoods.columns.get_level_values("Category")
```

```python
neighborhoods.head(1)
```

```python
neighborhoods.nunique()
```

## 7.3 Sorting a MultiIndex

```python
neighborhoods.sort_index()
```

```python
neighborhoods.sort_index(ascending = False).head()
```

```python
neighborhoods.sort_index(ascending = [True, False, True]).head()
```

```python
# The two lines below are equivalent
neighborhoods.sort_index(level = 1)
neighborhoods.sort_index(level = "City")
```

```python
# The two lines below are equivalent
neighborhoods.sort_index(level = [1, 2]).head()
neighborhoods.sort_index(level = ["City", "Street"]).head()
```

```python
neighborhoods.sort_index(
    level = ["City", "Street"], ascending = [True, False]
).head()
```

```python
# The two lines below are equivalent
neighborhoods.sort_index(axis = 1).head(3)
neighborhoods.sort_index(axis = "columns").head(3)
```

```python
neighborhoods.sort_index(
    axis = 1, level = "Subcategory", ascending = False
).head(3)
```

```python
neighborhoods = neighborhoods.sort_index(ascending = True)
```

```python
neighborhoods.head(3)
```

## 7.4 Selecting with a MultiIndex

```python
data = [
    [1, 2], 
    [3, 4]
]

df = pd.DataFrame(
    data = data,
    index = ["A", "B"],
    columns = ["X", "Y"]
)

df
```

```python
df["X"]
```

### 7.4.1 Extracting One or More Columns

```python
neighborhoods["Services"]
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# neighborhoods["Schools"]
```

```python
neighborhoods[("Services", "Schools")]
```

```python
neighborhoods[[("Services", "Schools"), ("Culture", "Museums")]]
```

```python
columns = [
    ("Services", "Schools"),
    ("Culture", "Museums")
]

neighborhoods[columns]
```

### 7.4.2 Extracting One or More Rows with loc

```python
df
```

```python
df.loc["A"]
```

```python
df.iloc[1]
```

```python
neighborhoods.loc[("TX", "Kingchester", "534 Gordon Falls")]
```

```python
neighborhoods.loc["CA"]
```

```python
neighborhoods.loc["CA", "Dustinmouth"]
```

```python
neighborhoods.loc["CA", "Culture"]
```

```python
neighborhoods.loc[("CA", "Dustinmouth")]
```

```python
neighborhoods.loc[("CA", "Dustinmouth"), ("Services",)]
```

```python
neighborhoods.loc[("CA", "Dustinmouth"), ("Services", "Schools")]
```

```python
neighborhoods["NE":"NH"]
```

```python
neighborhoods.loc[("NE", "Shawnchester"):("NH", "North Latoya")]
```

```python
start = ("NE", "Shawnchester")
end   = ("NH", "North Latoya")
neighborhoods.loc[start:end]
```

```python
neighborhoods.loc[("NE", "Shawnchester"):("NH")]
```

### 7.4.3 Extracting One or More Rows with iloc

```python
neighborhoods.iloc[25]
```

```python
neighborhoods.iloc[25, 2]
```

```python
neighborhoods.iloc[[25, 30]]
```

```python
neighborhoods.iloc[25:30]
```

```python
neighborhoods.iloc[25:30, 1:3]
```

```python
neighborhoods.iloc[-4:, -2:]
```

## 7.5 Cross Sections

```python
# The two lines below are equivalent
neighborhoods.xs(key = "Lake Nicole", level = 1)
neighborhoods.xs(key = "Lake Nicole", level = "City")
```

```python
neighborhoods.xs(
    axis = "columns", key = "Museums", level = "Subcategory"
).head()
```

```python
# The two lines below are equivalent
neighborhoods.xs(
    key = ("AK", "238 Andrew Rue"), level = ["State", "Street"]
)

neighborhoods.xs(
    key = ("AK", "238 Andrew Rue"), level = [0, 2]
)
```

## 7.6. Manipulating the Index


### 7.6.1 Resetting the Index

```python
neighborhoods.head()
```

```python
new_order = ["City", "State", "Street"]
neighborhoods.reorder_levels(order = new_order).head()
```

```python
neighborhoods.reorder_levels(order = [1, 0, 2]).head()
```

```python
neighborhoods.reset_index().tail()
```

```python
# The two lines below are equivalent
neighborhoods.reset_index(col_level = 1).tail()
neighborhoods.reset_index(col_level = "Subcategory").tail() 
```

```python
neighborhoods.reset_index(
    col_fill = "Address", col_level = "Subcategory"
).tail()
```

```python
neighborhoods.reset_index(level = "Street").tail()
```

```python
neighborhoods.reset_index(level = ["Street", "City"]).tail()
```

```python
neighborhoods.reset_index(level = "Street", drop = True).tail()
```

```python
neighborhoods = neighborhoods.reset_index()
```

### 7.6.2 Setting the Index

```python
neighborhoods.head(3)
```

```python
neighborhoods.set_index(keys = "City").head()
```

```python
neighborhoods.set_index(keys = ("Culture", "Museums")).head()
```

```python
neighborhoods.set_index(keys = ["State", "City"]).head()
```

## 7.7 Coding Challenge


### 7.7.1 Problems

```python
investments = pd.read_csv("investments.csv")
investments.head()
```

```python
investments.nunique()
```

```python
investments = investments.set_index(
    keys = ["Status", "Funding Rounds", "State"]
).sort_index()
```

```python
investments.head()
```

### 7.7.2 Solutions

```python
investments.loc[("Closed",)].head()
```

```python
investments.loc[("Acquired", 10)]
```

```python
investments.loc[("Operating", 6, "NJ")]
```

```python
investments.loc[("Closed", 8), ("Name",)]
```

```python
# The two lines below are equivalent
investments.xs(key = "NJ", level = 2).head()
investments.xs(key = "NJ", level = "State").head()
```

```python
investments = investments.reset_index()
investments.head()
```

## 7.8 Summary

```python

```
