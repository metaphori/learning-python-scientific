---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.6.0
  kernelspec:
    display_name: Python 3 (ipykernel)
    language: python
    name: python3
---

# Panda: Basics

```python pycharm={"is_executing": false}
import pandas as pd
m = pd.read_csv("../data/movies.csv")
m2 = pd.read_csv("../data/movies.csv", index_col="Rank")
print(type(m))
print(m)
print(m2)
```

Inspect a dataframe

```python pycharm={"name": "#%%\n", "is_executing": false}
print("Index:", m.index)
print("Index (m2):", m2.index)
print("Columns:", m.columns)
print("Len:", len(m))
print("Shape:", m.shape)
print("Size:", m.size)
print("Dtypes", m.dtypes)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Get some data from a dataframe
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
print(m.head(1))
print(m.tail(1))
print(m.to_numpy())
print(m.iloc[0])
print(m2.loc[1])
print(m.sort_values(by = "Year", ascending = True).iloc[0])
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Notice the type of a row
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
type(m.iloc[0])
```
