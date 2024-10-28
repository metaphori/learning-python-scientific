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

## 12.1 Reading from and Writing to JSON Files


### 12.1.1 Loading a JSON File into a DataFrame

```python
import pandas as pd
```

```python
nobel = pd.read_json("nobel.json")
nobel.head()
```

```python
nobel.loc[2, "prizes"]
```

```python
type(nobel.loc[2, "prizes"])
```

```python
chemistry_2019 = nobel.loc[0, "prizes"]
chemistry_2019
```

```python
pd.json_normalize(data = chemistry_2019)
```

```python
pd.json_normalize(data = chemistry_2019, record_path = "laureates")
```

```python
pd.json_normalize(
    data = chemistry_2019,
    record_path = "laureates",
    meta = ["year", "category"]
)
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# pd.json_normalize(
#     data = nobel["prizes"],
#     record_path = "laureates",
#     meta = ["year", "category"]
# )
```

```python
cheese_consumption = {
    "France": 57.9,
    "Germany": 53.2,
    "Luxembourg": 53.2
}
```

```python
cheese_consumption.setdefault("France", 100)
```

```python
cheese_consumption["France"]
```

```python
cheese_consumption.setdefault("Italy", 48)
```

```python
cheese_consumption
```

```python
def add_laureates_key(entry):
    entry.setdefault("laureates", [])

nobel["prizes"].apply(add_laureates_key)
```

```python
winners = pd.json_normalize(
    data = nobel["prizes"],
    record_path = "laureates",
    meta = ["year", "category"]
)

winners
```

### 12.1.2 Exporting a DataFrame to a JSON File

```python
winners.head(2)
```

```python
winners.head(2).to_json(orient = "records")
```

```python
winners.head(2).to_json(orient = "split")
```

```python
winners.to_json("winners.json", orient = "records")
```

## 12.2 Reading from and Writing to CSV Files

```python
url = "https://data.cityofnewyork.us/api/views/25th-nujf/rows.csv"
baby_names = pd.read_csv(url)
baby_names.head()
```

```python
baby_names.head(10).to_csv()
```

```python
baby_names.head(10).to_csv(index = False)
```

```python
baby_names.to_csv("NYC_Baby_Names.csv", index = False)
```

```python
baby_names.to_csv(
    "NYC_Baby_Names.csv",
    index = False, 
    columns = ["Gender", "Child's First Name", "Count"]
)
```

## 12.3 Reading from and Writing to Excel Workbooks

```python
pd.read_excel("Single Worksheet.xlsx")
```

```python
pd.read_excel(
    io = "Single Worksheet.xlsx",
    usecols = ["City", "First Name", "Last Name"],
    index_col = "City"
)
```

```python
pd.read_excel("Multiple Worksheets.xlsx")
```

```python
# The two lines below are equivalent
pd.read_excel("Multiple Worksheets.xlsx", sheet_name = 0)
pd.read_excel("Multiple Worksheets.xlsx", sheet_name = "Data 1")
```

```python
workbook = pd.read_excel(
    "Multiple Worksheets.xlsx", sheet_name = None
)

workbook
```

```python
type(workbook)
```

```python
workbook["Data 2"]
```

```python
pd.read_excel(
    "Multiple Worksheets.xlsx",
    sheet_name = ["Data 1", "Data 3"]
)
```

```python
pd.read_excel("Multiple Worksheets.xlsx", sheet_name = [1, 2])
```

### 12.3.3 Exporting Excel Workbooks

```python
baby_names.head()
```

```python
girls = baby_names[baby_names["Gender"] == "FEMALE"]
boys = baby_names[baby_names["Gender"] == "MALE"]
```

```python
excel_file = pd.ExcelWriter("Baby_Names.xlsx")
excel_file
```

```python
girls.to_excel(
    excel_writer = excel_file, sheet_name = "Girls", index = False
)
```

```python
boys.to_excel(
    excel_file,
    sheet_name = "Boys",
    index = False,
    columns = ["Child's First Name", "Count", "Rank"]
)
```

```python
excel_file.save()
```

## 12.4 Coding Challenge

```python
tv_shows_json = pd.read_json("tv_shows.json")
tv_shows_json
```

```python
tv_shows_json.loc[0, "shows"]
```

### 12.4.1 Problems


### 12.4.2 Solutions

```python
tv_shows = pd.json_normalize(
    data = tv_shows_json["shows"],
    record_path = "episodes",
    meta = ["show", "runtime", "network"]
)

tv_shows
```

```python
xfiles = tv_shows[tv_shows["show"] == "The X-Files"]
lost = tv_shows[tv_shows["show"] == "Lost"]
buffy = tv_shows[tv_shows["show"] == "Buffy the Vampire Slayer"]
```

```python
episodes = pd.ExcelWriter("episodes.xlsx")
episodes
```

```python
xfiles.to_excel(
    excel_writer = episodes, sheet_name = "X-Files", index = False
)
```

```python
lost.to_excel(
    excel_writer = episodes, sheet_name = "Lost", index = False
)
```

```python
buffy.to_excel(
    excel_writer = episodes,
    sheet_name = "Buffy the Vampire Slayer",
    index = False
)
```

```python
episodes.save()
```

## 12.5 Summary

```python

```
