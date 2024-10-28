---
jupyter:
  jupytext:
    formats: ipynb,md
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.14.1
  kernelspec:
    display_name: mooc
    language: python
    name: python3
---

These notes are based on my latex notes (`python-scientific-pandas`) which in turn are based on book "Pandas in Action", which has an associated repository: 


# Pandas in 10 minutes

Reference: [10 minutes to pandas](https://pandas.pydata.org/docs/user_guide/10min.html)


#### Conventional imports

```python
import numpy as np

import pandas as pd
```

### Basic data structures

Pandas provides two types of classes for handling data:

1. `Series`: a one-dimensional labeled array holding data of any type (such as integers, strings, Python objects etc.)
2. `DataFrame`: a two-dimensional data structure that holds data like a two-dimension array or a **table** with rows and columns.
    - there are two **axes**: the **index** (axis 0) and **columns** (axis 1)


#### Series: creation

Creating a `Series` by passing a **list of values**, letting pandas create a default **`RangeIndex`**.

```python
s = pd.Series([1, 3, 5, np.nan, 6, 8])
s
```

#### DataFrame: creation

Creating a `DataFrame` by passing a NumPy array with a datetime index using **`date_range()`** and labeled columns:

```python
dates = pd.date_range('20130101', periods=6)
print(dates)
df = pd.DataFrame(np.random.randn(6, 4), index=dates, columns=list("ABCD"))
df
```

Creating a DataFrame by **passing a dictionary** of objects where the keys are the column labels and the values are the column values.

```python
df2 = pd.DataFrame(
    {
        "A": 1.0,
        "B": pd.Timestamp("20130102"),
        "C": pd.Series(1, index=list(range(4)), dtype="float32"),
        "D": np.array([1,2,3,4], dtype="int32"),
        "E": pd.Categorical(["test", "train", "test", "train"]),
        "F": "foo",
    }
)
df2
```

The columns of the resulting DataFrame have different dtypes:

```python
df2.dtypes
```

### Viewing data


**`DataFrame.describe()`** shows a quick statistic summary of your data:

```python
df.describe()
```

Use `DataFrame.head()` and `DataFrame.tail()` to view the top and bottom rows of the frame respectively

```python
print(
    df.head(), '\n',
    df.tail(2)
)
```

Display the `DataFrame.index` or `DataFrame.columns`

```python
print(
    df, 
    '\n\nShape = ', df.shape, 
    '\nIndex = \n', df.index,
    '\nColumns = \n', df.columns
)
```

Return a NumPy representation of the **underlying data  with `DataFrame.to_numpy()` i.e. without the index or column labels**:

```python
df.to_numpy()
```

* NumPy arrays have one dtype for the entire array while **pandas DataFrames have one dtype per column**. 
    * When you call `DataFrame.to_numpy()`, pandas will find the **NumPy dtype that can hold all of the dtypes in the DataFrame**. If the common data type is **object**, `DataFrame.to_numpy()` will **require copying data (!!!)**.

```python
df.dtypes
```

**Sorting.** `DataFrame.sort_index(axis=0, ascending=True)` sorts by an axis (remember: axis 0 denotes the index, and axis 1 the columns). `DataFrame.sort_values(by=C)` sorts by value.

```python
print(
    df, '\n\n',
    'Sorted by axis=1 (i.e. COLUMNS) DESC\n', 
    df.sort_index(axis=1, ascending=False), '\n\n',
    'Sorted by values of B\n',
    df.sort_values(by="B")
)
```

### Selection of data

- Get item `[]`
- Selection by label: `loc()`, `at()`
- Selection by position: `iloc()`, `iat()`
- Boolean indexing: e.g. `df[df["A"]>0]`
- Setting values: e.g., `df["F"] = series1`
- Note: While standard Python / NumPy expressions for selecting and setting are intuitive and come in handy for interactive work, for production code, we recommend the optimized pandas data access methods, DataFrame.at(), DataFrame.iat(), DataFrame.loc() and DataFrame.iloc().


#### GetItem `[]`

For a `DataFrame`, passing a **single label** as in `df["A"]` **selects a columns and yields a `Series`** (in this case, equivalent to `df.A`):

```python
df["A"] # same as df.A; it selects the  column
```

For a DataFrame, passing a **slice `:` selects matching rows**:


```python
df[0:3] # selects rows
```

# Panda: Basics (Inspecting Movies)

```python pycharm={"is_executing": false}
# import pandas as pd
m = pd.read_csv("../data/movies.csv")
m2 = pd.read_csv("../data/movies.csv", index_col="Rank")
print(type(m))
print(m)
print(m2)
```

## DataFrames

### Inspect a dataframe

```python pycharm={"is_executing": false, "name": "#%%\n"}
print("Index:", m.index)
print("Index (m2):", m2.index)
print("Columns:", m.columns)
print("Len:", len(m))
print("Shape:", m.shape)
print("Size:", m.size)
print("Dtypes", m.dtypes)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
### Get some data from a dataframe
<!-- #endregion -->

```python pycharm={"is_executing": false, "name": "#%%\n"}
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

```python pycharm={"is_executing": false, "name": "#%%\n"}
type(m.iloc[0])
```

<!-- #region pycharm={"name": "#%% md\n"} -->
### Analyse a dataframe
<!-- #endregion -->

```python pycharm={"is_executing": false, "name": "#%%\n"}
studios_occurrences = m["Studio"].value_counts()
print(type(studios_occurrences))
print(studios_occurrences)
```

<!-- #region pycharm={"name": "#%% md\n"} -->
### Filtering
<!-- #endregion -->

```python pycharm={"is_executing": false, "name": "#%%\n"}
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

### Bulk change

```python pycharm={"is_executing": false, "name": "#%%\n"}
m["Gross"] = (
  m["Gross"].astype('str')
  .str.replace("$", "", regex = False)
  .str.replace(",", "", regex = False)
  .astype(float)
)
m
```

<!-- #region pycharm={"name": "#%% md\n"} -->
### Grouping data
<!-- #endregion -->

```python pycharm={"is_executing": false, "name": "#%%\n"}
studios = m.groupby("Studio") # type: pandas.core.groupby.generic.DataFrameGroupBy
print(type(studios))
studios_with_higher_gross = studios["Gross"].sum().sort_values(ascending = False).head()
studios_with_higher_gross
```

<!-- #region pycharm={"name": "#%% md\n"} -->
## Series
<!-- #endregion -->

```python pycharm={"is_executing": false, "name": "#%%\n"}
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
### Inspecting
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
### Reading data
<!-- #endregion -->

```python pycharm={"is_executing": false, "name": "#%%\n"}
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

```python pycharm={"is_executing": false, "name": "#%%\n"}
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
