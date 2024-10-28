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

## 8.1 Wide vs. Narrow Data


## 8.2 Creating a Pivot Table from a DataFrame

```python
import pandas as pd
```

```python
pd.read_csv("sales_by_employee.csv").head()
```

```python
sales = pd.read_csv("sales_by_employee.csv",
                    parse_dates = ["Date"])

sales.tail()
```

### 8.2.1 The pivot_table Method

```python
sales.pivot_table(index = "Date")
```

```python
sales.pivot_table(index = "Date", aggfunc = "mean")
```

```python
sales.pivot_table(index = "Date", aggfunc = "sum")
```

```python
sales.pivot_table(
    index = "Date", values = "Revenue", aggfunc = "sum"
)
```

```python
sales.pivot_table(
    index = "Date",
    columns = "Name",
    values = "Revenue",
    aggfunc = "sum"
)
```

```python
sales.pivot_table(
    index = "Date", 
    columns = "Name", 
    values = "Revenue", 
    aggfunc = "sum",
    fill_value = 0
)
```

```python
sales.pivot_table(
    index = "Date",
    columns = "Name",
    values = "Revenue",
    aggfunc = "sum",
    fill_value = 0,
    margins = True
)
```

```python
sales.pivot_table(
    index = "Date", 
    columns = "Name", 
    values = "Revenue", 
    aggfunc = "sum",
    fill_value = 0,
    margins = True,
    margins_name = "Total"
)
```

### 8.2.2 Additional Options for Pivot Tables

```python
sales.pivot_table(
    index = "Date", 
    columns = "Name",
    values = "Revenue",
    aggfunc = "count"
)
```

```python
sales.pivot_table(
    index = "Date",
    columns = "Name",
    values = "Revenue",
    aggfunc = ["sum", "count"],
    fill_value = 0
)
```

```python
sales.pivot_table(
    index = "Date",
    columns = "Name", 
    values = ["Revenue", "Expenses"],
    fill_value = 0,
    aggfunc = { "Revenue": "min", "Expenses": "min" }
)
```

```python
sales.pivot_table(
    index = ["Name", "Date"], values = "Revenue", aggfunc = "sum"
).head(10)
```

```python
sales.pivot_table(
    index = ["Date", "Name"], values = "Revenue", aggfunc = "sum"
).head(10)
```

## 8.3 Stacking and Unstacking Index Levels

```python
sales.head()
```

```python
by_name_and_date = sales.pivot_table(
    index = "Name", 
    columns = "Date", 
    values = "Revenue", 
    aggfunc = "sum"
)

by_name_and_date.head(2) 
```

```python
by_name_and_date.stack().head(7)
```

```python
sales_by_customer = sales.pivot_table(
    index = ["Customer", "Name"],
    values = "Revenue",
    aggfunc = "sum"
)

sales_by_customer.head()
```

```python
sales_by_customer.unstack()
```

## 8.4 Melting a Data Set

```python
sales.head(1)
```

```python
video_game_sales = pd.read_csv("video_game_sales.csv")
video_game_sales.head()
```

```python
video_game_sales.head(1)
```

```python
video_game_sales.melt(id_vars = 'Name', value_vars = "NA").head()
```

```python
regional_sales_columns = ["NA", "EU", "JP", "Other"]

video_game_sales.melt(
    id_vars = "Name", value_vars = regional_sales_columns
)
```

```python
video_game_sales_by_region = video_game_sales.melt(
    id_vars = "Name", 
    value_vars = regional_sales_columns,
    var_name = "Region",
    value_name = "Sales"
)

video_game_sales_by_region.head()
```

```python
video_game_sales_by_region.pivot_table(
    index = "Name", values = "Sales", aggfunc = "sum"
).head()
```

## 8.5 Exploding a List of Values

```python
recipes = pd.read_csv("recipes.csv")
recipes
```

```python
recipes["Ingredients"].str.split(",")
```

```python
recipes["Ingredients"] = recipes["Ingredients"].str.split(",")
recipes
```

```python
recipes.explode("Ingredients")
```

## 8.6 Coding Challenge


### 8.6.1 Problems

```python
cars = pd.read_csv("used_cars.csv")
cars.head()
```

```python
min_wage = pd.read_csv("minimum_wage.csv")
min_wage.head()
```

### 8.6.2 Solutions

```python
cars.pivot_table(
    values = "Price", index = "Fuel", aggfunc = "sum"
)
```

```python
cars.pivot_table(
    values = "Price",
    index = "Manufacturer",
    columns = "Transmission",
    aggfunc = "count",
    margins = True
).tail()
```

```python
cars.pivot_table(
    values = "Price",
    index = ["Year", "Fuel"],
    columns = ["Transmission"],
    aggfunc = "mean"
)
```

```python
report = cars.pivot_table(
    values = "Price",
    index = ["Year", "Fuel"],
    columns = ["Transmission"],
    aggfunc = "mean"
)
```

```python
report.stack()
```

```python
year_columns = [
    "2010", "2011", "2012", "2013", 
    "2014", "2015", "2016", "2017"
]

min_wage.melt(id_vars = "State", value_vars = year_columns)
```

```python
min_wage.melt(id_vars = "State")
```

```python
min_wage.melt(
    id_vars = "State", var_name = "Year", value_name = "Wage"
)
```

## 8.7 Summary

```python

```
