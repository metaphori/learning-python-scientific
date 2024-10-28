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

## 14.1 Installing matplotlib


## 14.2 Line Charts

```python
import pandas as pd
import matplotlib.pyplot as plt
%matplotlib inline
```

```python
pd.read_csv("space_missions.csv").head()
```

```python
space = pd.read_csv(
    "space_missions.csv",
    parse_dates = ["Date"],
    index_col = "Date"
)

space.head()
```

```python
space["Cost"].head()
```

```python
space["Cost"].plot()
```

```python
space.plot()
```

```python
data = [
    [2000, 3000000],
    [5000, 5000000]
]

df = pd.DataFrame(data = data, columns = ["Small", "Large"])
df
```

```python
df.plot()
```

```python
space.plot(y = "Cost")
```

```python
space.plot(y = "Cost", colormap = "gray")
```

```python
print(plt.colormaps())
```

## 14.3 Bar Graphs

```python
space["Company Name"].value_counts()
```

```python
space["Company Name"].value_counts().plot(kind = "bar")
```

```python
space["Company Name"].value_counts().plot(kind = "barh")
```

## 14.4 Pie Charts

```python
space["Status"].value_counts()
```

```python
space["Status"].value_counts().plot(kind = "pie")
```

```python
space["Status"].value_counts().plot(kind = "pie", legend = True)
```

## 14.5 Summary

```python

```
