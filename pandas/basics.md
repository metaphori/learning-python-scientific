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

## DataFrames

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
print(m["Studio"]) # extract a column as a Series (keeping the index)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Notice the type of a row
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
type(m.iloc[0])
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Analyse a dataframe
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
studios_occurrences = m["Studio"].value_counts()
print(type(studios_occurrences))
print(studios_occurrences)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Filtering:
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
print(m[m["Studio"] == "Universal"])

released_by_universal = m["Studio"] == "Universal"
released_in_2015 = m["Year"] == 2015
print(m[released_by_universal & released_in_2015])

before_1975 = m["Year"] < 1975
movies_before_1975 = m[before_1975]
print(movies_before_1975)
print(m[m["Year"].between(1983, 1986)])

has_dark_in_title = m["Title"].str.lower().str.contains("dark")
print(m[has_dark_in_title])
```

Bulk change

```python pycharm={"name": "#%%\n", "is_executing": false}
m["Gross"] = (
  m["Gross"].astype('str')
  .str.replace("$", "", regex = False)
  .str.replace(",", "", regex = False)
  .astype(float)
)
m
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Grouping data:
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
studios = m.groupby("Studio") # type: pandas.core.groupby.generic.DataFrameGroupBy
print(type(studios))
studios_with_higher_gross = studios["Gross"].sum().sort_values(ascending = False).head()
studios_with_higher_gross
```

<!-- #region pycharm={"name": "#%% md\n"} -->
## Series
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
import numpy as np

s = pd.Series() #  Series([], dtype: object)
s = pd.Series(["Bob", "Rick"]) # 0   Bob ; 1   Rick ; dtype: object
s = pd.Series(data = ["Bob", "Rick", "Mark"], index = [1, 5, 7])
s = pd.Series([5, 3, np.nan, 9], [0, 1, 2, 3], dtype="float")
d = { "Rick": 77, "Bob": 44 }
s = pd.Series(d) # using a dict: keys are labels
print(s)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Inspecting
<!-- #endregion -->

```python pycharm={"name": "#%%\n"}
print(s.values, type(s.values))
print(s.index, type(s.index))
print(s.dtype)
print(s.shape)
print(pd.Series(data = [3, 3]).is_unique, pd.Series(data = [3, 5]).is_unique)
print(s.is_monotonic_decreasing)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
Reading data
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
s = pd.Series(range(0,500,5))
print(s.head())
print(s.tail())

s = pd.Series(range(0,10,2))
s[s.index % 2 == 0] = np.nan
print(s)
print(s.sum()) # 8.0
print(s.sum(skipna = False))

print(pd.Series(range(0,10,2)).cumsum()) # 0 0; 1 2; 2 6; 3 12; 4 20

s = pd.Series([1,2,3])
s = s * 2 # 0 2 ; 1 4; 2 6
print(s)

s1 = pd.Series([1,2,3], ['A','B','C'])
s2 = pd.Series([1,2,3], ['C','A','B'])
print(s1 + s2)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
### Challenge (sec 2.7.1 'Pandas in Action')
<!-- #endregion -->

```python pycharm={"name": "#%%\n", "is_executing": false}
superheroes = [
 "Batman",
 "Superman",
 "Spider-Man",
 "Iron Man",
 "Captain America",
 "Wonder Woman"
 ]
strength_levels = (100, 120, 90, 95, 110, 120)

s1 = pd.Series(superheroes)
s2 = pd.Series(strength_levels)
heroes = pd.Series(strength_levels, superheroes)
print(heroes.head(2))
print(heroes.tail(4))
print(len(heroes.unique()))
print(heroes.mean())
print(heroes.min(), heroes.max())
print(heroes * 2)
print(dict(heroes))
```
