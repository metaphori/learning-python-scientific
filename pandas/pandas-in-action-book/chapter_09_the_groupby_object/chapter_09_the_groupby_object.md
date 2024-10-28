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

## 9.1 Creating a GroupBy Object from Scratch

```python
import pandas as pd
```

```python
food_data = {
    "Item": ["Banana", "Cucumber", "Orange", "Tomato", "Watermelon"],
    "Type": ["Fruit", "Vegetable", "Fruit", "Vegetable", "Fruit"],
    "Price": [0.99, 1.25, 0.25, 0.33, 3.00]
}

supermarket = pd.DataFrame(data = food_data)

supermarket
```

```python
groups = supermarket.groupby("Type")
groups
```

```python
groups.get_group("Fruit")
```

```python
groups.get_group("Vegetable")
```

```python
groups.mean()
```

## 9.2 Creating a GroupBy Object from a Data Set

```python
fortune = pd.read_csv("fortune1000.csv")
fortune
```

```python
in_retailing = fortune["Sector"] == "Retailing"
retail_companies = fortune[in_retailing]
retail_companies.head()
```

```python
retail_companies["Revenues"].head()
```

```python
retail_companies["Revenues"].mean()
```

```python
sectors = fortune.groupby("Sector")
```

```python
sectors
```

```python
len(sectors)
```

```python
fortune["Sector"].nunique()
```

```python
sectors.size()
```

## 9.3 Attributes and Methods on a GroupBy Object

```python
sectors.groups
```

```python
fortune.loc[26, "Sector"]
```

```python
sectors.first()
```

```python
sectors.last()
```

```python
sectors.nth(0)
```

```python
sectors.nth(3)
```

```python
fortune[fortune["Sector"] == "Apparel"].head()
```

```python
sectors.head(2)
```

```python
sectors.tail(3)
```

```python
sectors.get_group("Energy").head()
```

## 9.4 Aggregate Operations

```python
sectors.sum().head(10)
```

```python
sectors.get_group("Aerospace & Defense").head()
```

```python
sectors.get_group("Aerospace & Defense").loc[:,"Revenues"].head()
```

```python
sectors.get_group("Aerospace & Defense").loc[:, "Revenues"].sum()
```

```python
sectors.mean().head()
```

```python
sectors["Revenues"]
```

```python
sectors["Revenues"].sum().head()
```

```python
sectors["Employees"].mean().head()
```

```python
sectors["Profits"].max().head()
```

```python
sectors["Employees"].min().head()
```

### 9.4.1 Applying Multiple Aggregations using the agg Method

```python
aggregations = {
    "Revenues": "min",
    "Profits": "max",
    "Employees": "mean"
}

sectors.agg(aggregations).head()
```

## 9.5 Applying a Custom Operation to all Groups

```python
fortune.nlargest(n = 5, columns = "Profits")
```

```python
def get_largest_row(df):
    return df.nlargest(1, "Revenues")
```

```python
sectors.apply(get_largest_row).head()
```

## 9.6 Grouping by Multiple Columns

```python
sector_and_industry = fortune.groupby(by = ["Sector", "Industry"])
```

```python
sector_and_industry.size()
```

```python
sector_and_industry.get_group(("Business Services", "Education"))
```

```python
sector_and_industry.sum().head()
```

```python
sector_and_industry["Revenues"].mean().head(5)
```

## 9.7 Coding Challenge

```python
cereals = pd.read_csv("cereals.csv")
cereals.head()
```

### 9.7.1 Problems


### 9.7.2 Solutions

```python
manufacturers = cereals.groupby("Manufacturer")
```

```python
len(manufacturers)
```

```python
manufacturers.size()
```

```python
manufacturers.get_group("Nabisco")
```

```python
manufacturers.mean()
```

```python
manufacturers["Sugars"].max()
```

```python
manufacturers["Fiber"].min()
```

```python
def smallest_sugar_row(df):
    return df.nsmallest(1, "Sugars")
```

```python
manufacturers.apply(smallest_sugar_row)
```

## 9.8 Summary

```python

```
