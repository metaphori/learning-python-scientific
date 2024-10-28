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

## 1.1 Data in the 21st Century


## 1.2 Introducing pandas


### 1.2.1 Pandas vs. Graphical Spreadsheet Applications


### 1.2.2 Pandas vs. Its Competitors


## 1.3 A Tour of Pandas


### 1.3.1 Importing a Data Set

```python
import pandas as pd
```

```python
pd.read_csv("movies.csv")
```

```python
pd.read_csv("movies.csv", index_col = "Title")
```

```python
movies = pd.read_csv("movies.csv", index_col = "Title")
```

### 1.3.2 Manipulating a DataFrame

```python
movies.head(4)
```

```python
movies.tail(6)
```

```python
len(movies)
```

```python
movies.shape
```

```python
movies.size
```

```python
movies.dtypes
```

```python
movies.iloc[499]
```

```python
movies.loc["Forrest Gump"]
```

```python
movies.loc["101 Dalmatians"]
```

```python
movies.sort_values(by = "Year", ascending = False).head()
```

```python
movies.sort_values(by = ["Studio", "Year"]).head()
```

```python
movies.sort_index().head()
```

### 1.3.3 Counting Values in a Series

```python
movies["Studio"]
```

```python
movies["Studio"].value_counts().head(10)
```

### 1.3.4	Filtering a Column by One or More Criteria

```python
movies[movies["Studio"] == "Universal"]
```

```python
released_by_universal = (movies["Studio"] == "Universal")
movies[released_by_universal].head()
```

```python
released_by_universal = movies["Studio"] == "Universal"
released_in_2015 = movies["Year"] == 2015
movies[released_by_universal & released_in_2015]
```

```python
released_by_universal = movies["Studio"] == "Universal"
released_in_2015 = movies["Year"] == 2015
movies[released_by_universal | released_in_2015]
```

```python
before_1975 = movies["Year"] < 1975
movies[before_1975]
```

```python
mid_80s = movies["Year"].between(1983, 1986)
movies[mid_80s]
```

```python
has_dark_in_title = movies.index.str.lower().str.contains("dark")
movies[has_dark_in_title]
```

### 1.3.5 Grouping Data

```python
movies["Gross"].str.replace(
    "$", "", regex = False
).str.replace(",", "", regex = False)
```

```python
(
    movies["Gross"]
    .str.replace("$", "", regex = False)
    .str.replace(",", "", regex = False)
    .astype(float)
)
```

```python
movies["Gross"] = (
    movies["Gross"]
    .str.replace("$", "", regex = False)
    .str.replace(",", "", regex = False)
    .astype(float)
)
```

```python
movies["Gross"].mean()
```

```python
studios = movies.groupby("Studio")
```

```python
studios["Gross"].count().head()
```

```python
studios["Gross"].count().sort_values(ascending = False).head()
```

```python
studios["Gross"].sum().head()
```

```python
studios["Gross"].sum().sort_values(ascending = False).head()
```

## 1.4 Summary

```python

```
