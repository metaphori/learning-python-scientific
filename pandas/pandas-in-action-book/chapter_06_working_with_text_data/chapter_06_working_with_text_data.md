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

## 6.1 Letter Casing and Whitespace

```python
import pandas as pd
```

```python
inspections = pd.read_csv("chicago_food_inspections.csv")
inspections
```

```python
inspections["Name"].head()
```

```python
inspections["Name"].head().values
```

```python
inspections["Name"].str
```

```python
dessert = "  cheesecake  "
dessert.lstrip()
```

```python
dessert.rstrip()
```

```python
dessert.strip()
```

```python
inspections["Name"].str.lstrip().head()
```

```python
inspections["Name"].str.rstrip().head()
```

```python
inspections["Name"].str.strip().head()
```

```python
inspections["Name"] = inspections["Name"].str.strip()
```

```python
inspections.columns
```

```python
for column in inspections.columns:
    inspections[column] = inspections[column].str.strip()
```

```python
inspections["Name"].str.lower().head()
```

```python
steaks = pd.Series(["porterhouse", "filet mignon", "ribeye"])
steaks
```

```python
steaks.str.upper()
```

```python
inspections["Name"].str.capitalize().head()
```

```python
inspections["Name"].str.title().head()
```

## 6.2 String Slicing

```python
inspections["Risk"].head()
```

```python
len(inspections)
```

```python
inspections["Risk"].unique()
```

```python
inspections = inspections.dropna(subset = ["Risk"])
```

```python
inspections["Risk"].unique()
```

```python
inspections = inspections.replace(
    to_replace = "All", value = "Risk 4 (Extreme)"
)
```

```python
inspections["Risk"].unique()
```

### 6.2.1 String Slicing and Character Replacement

```python
inspections["Risk"].str.slice(5, 6).head()
```

```python
inspections["Risk"].str[5:6].head()
```

```python
inspections["Risk"].str.slice(8).head()
```

```python
inspections["Risk"].str[8:].head()
```

```python
inspections["Risk"].str.slice(8, -1).head()
```

```python
inspections["Risk"].str[8:-1].head()
```

```python
inspections["Risk"].str.slice(8).str.replace(")", "").head()
```

## 6.3 Boolean Methods

```python
"Pizza" in "Jets Pizza"
```

```python
"pizza" in "Jets Pizza"
```

```python
inspections["Name"].str.lower().str.contains("pizza").head()
```

```python
has_pizza = inspections["Name"].str.lower().str.contains("pizza")
inspections[has_pizza]
```

```python
inspections["Name"].str.lower().str.startswith("tacos").head()
```

```python
starts_with_tacos = (
    inspections["Name"].str.lower().str.startswith("tacos")
)
inspections[starts_with_tacos]
```

```python
ends_with_tacos = (
    inspections["Name"].str.lower().str.endswith("tacos")
)
inspections[ends_with_tacos]
```

## 6.4 Splitting Strings

```python
customers = pd.read_csv("customers.csv")
customers.head()
```

```python
customers["Name"].str.len().head()
```

```python
phone_number = "555-123-4567"
phone_number.split("-")
```

```python
# The two lines below are equivalent
customers["Name"].str.split(pat = " ").head()
customers["Name"].str.split(" ").head()
```

```python
customers["Name"].str.split(" ").str.len().head()
```

```python
customers["Name"].str.split(pat = " ", n = 1).head()
```

```python
customers["Name"].str.split(pat = " ", n = 1).str.get(0).head()
```

```python
customers["Name"].str.split(pat = " ", n = 1).str.get(1).head()
```

```python
customers["Name"].str.split(pat = " ", n = 1).str.get(-1).head()
```

```python
customers["Name"].str.split(
    pat = " ", n = 1, expand = True
).head()
```

```python
customers["Name"].str.split(pat = " ", expand = True).head()
```

```python
customers[["First Name", "Last Name"]] = customers[
    "Name"
].str.split(pat = " ", n = 1, expand = True)
```

```python
customers
```

```python
customers = customers.drop(labels = "Name", axis = "columns")
```

```python
customers.head()
```

## 6.5 Coding Challenge


### 6.5.1 Problems


### 6.5.2 Solutions

```python
customers["Address"].str.split(",").head()
```

```python
customers["Address"].str.split(", ").head()
```

```python
customers["Address"].str.split(", ", expand = True).head()
```

```python
new_cols = ["Street", "City", "State", "Zip"]

customers[new_cols] = customers["Address"].str.split(
    pat = ", ", expand = True
)
```

```python
customers.drop(labels = "Address", axis = "columns").head()
```

```python
del customers["Address"]
```

```python
customers.tail()
```

## 6.6 A Note on Regular Expressions

```python
customers["Street"].head()
```

```python
customers["Street"].str.replace(
    "\d{4,}", "*", regex = True
).head()
```

## 6.7 Summary

```python

```
