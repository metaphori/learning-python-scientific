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

## 10.1 Introducing the Data Sets

```python
import pandas as pd
```

```python
groups1 = pd.read_csv("meetup/groups1.csv")
groups1.head()
```

```python
groups2 = pd.read_csv("meetup/groups2.csv")
groups2.head()
```

```python
categories = pd.read_csv("meetup/categories.csv")
categories.head()
```

```python
pd.read_csv("meetup/cities.csv").head()
```

```python
cities = pd.read_csv(
    "meetup/cities.csv", dtype = {"zip": "string"}
)
cities.head()
```

## 10.2 Concatenating Data Sets

```python
pd.concat(objs = [groups1, groups2])
```

```python
len(groups1)
```

```python
len(groups2)
```

```python
len(groups1) + len(groups2)
```

```python
pd.concat(objs = [groups1, groups2], ignore_index = True)
```

```python
pd.concat(objs = [groups1, groups2], keys = ["G1", "G2"])
```

```python
groups = pd.concat(objs = [groups1, groups2], ignore_index = True)
```

### 10.2.1 Missing Values in Combined DataFrames

```python
sports_champions_A = pd.DataFrame(
    data = [
        ["New England Patriots", "Houston Astros"],
        ["Philadelphia Eagles", "Boston Red Sox"]
    ],
    columns = ["Football", "Baseball"],
    index = [2017, 2018]
)

sports_champions_A
```

```python
sports_champions_B = pd.DataFrame(
    data = [
        ["New England Patriots", "St. Louis Blues"],
        ["Kansas City Chiefs", "Tampa Bay Lightning"]
    ],
    columns = ["Football", "Hockey"],
    index = [2019, 2020]
)
```

```python
pd.concat(objs = [sports_champions_A, sports_champions_B])
```

```python
sports_champions_C = pd.DataFrame(
    data = [
        ["Pittsburgh Penguins", "Golden State Warriors"],
        ["Washington Capitals", "Golden State Warriors"]
    ],
    columns = ["Hockey", "Basketball"],
    index = [2017, 2018]
)

```

```python
pd.concat(objs = [sports_champions_A, sports_champions_C])
```

```python
pd.concat(
    objs = [sports_champions_A, sports_champions_C], 
    axis = 1
)
pd.concat(
    objs = [sports_champions_A, sports_champions_C], axis = "columns"
)
```

## 10.3 Left Joins

```python
groups.head(3)
```

```python
categories.head(3)
```

```python
groups.merge(categories, how = "left", on = "category_id").head()
```

## 10.4 Inner Joins

```python
groups.head(3)
```

```python
categories.head(3)
```

```python
groups.merge(categories, how = "inner", on = "category_id")
```

```python
groups[groups["category_id"] == 14]
```

```python
categories[categories["category_id"] == 14]
```

## 10.5 Outer Joins

```python
groups.head(3)
```

```python
cities.head(3)
```

```python
groups.merge(
    cities, how = "outer", left_on = "city_id", right_on = "id"
)
```

```python
groups.merge(
    cities,
    how = "outer",
    left_on = "city_id",
    right_on = "id",
    indicator = True
)
```

```python
outer_join = groups.merge(
    cities,
    how = "outer",
    left_on = "city_id",
    right_on = "id",
    indicator = True
)

in_right_only = outer_join["_merge"] == "right_only"

outer_join[in_right_only].head()
```

## 10.6 Merging on Index Labels

```python
cities.head(3)
```

```python
cities = cities.set_index("id")
```

```python
cities.head(3)
```

```python
groups.head(3)
```

```python
groups.merge(
    cities,
    how = "left",
    left_on = "city_id",
    right_index = True
)
```

## 10.7 Coding Challenge

```python
pd.read_csv("restaurant/week_1_sales.csv").head()
```

```python
week1 = pd.read_csv("restaurant/week_1_sales.csv")
week2 = pd.read_csv("restaurant/week_2_sales.csv")
```

```python
pd.read_csv("restaurant/customers.csv", index_col = "ID").head()
```

```python
customers = pd.read_csv(
    "restaurant/customers.csv", index_col = "ID"
)
```

```python
pd.read_csv("restaurant/foods.csv", index_col = "Food ID")
```

```python
foods = pd.read_csv("restaurant/foods.csv", index_col = "Food ID")
```

### 10.7.1 Problems


### 10.7.2 Solutions

```python
pd.concat(objs = [week1, week2], keys = ["Week 1", "Week 2"])
```

```python
week1.merge(
    right = week2, how = "inner", on = "Customer ID"
).head()
```

```python
week1.merge(
    right = week2, how = "inner", on = "Customer ID"
).drop_duplicates(subset = ["Customer ID"]).head()
```

```python
week1.merge(
    right = week2,
    how = "inner",
    on = ["Customer ID", "Food ID"]
)
```

```python
week1.merge(
    right = week2,
    how = "outer",
    on = "Customer ID",
    indicator = True
).head()
```

```python
week1.merge(
    right = customers,
    how = "left",
    left_on = "Customer ID",
    right_index = True
).head()
```

## 10.8 Summary

```python

```
