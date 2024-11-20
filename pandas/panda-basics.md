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

```python
# other imports for the examples
import math
import matplotlib.pyplot as plt
```

### Basic data structures

Pandas provides two types of classes for handling data:

1. `Series`: a one-dimensional labeled array holding data of any type (such as integers, strings, Python objects etc.)
    - More precisely: a **1-dim ndarray with axis labels** (including **time series**)
    - Main properties: `index`, `values`, `dtype`, `shape` (which will be `(n,)`), `axes`, `name`, `ndim` (1 by definition)
2. `DataFrame`: a **two-dimensional** data structure that holds data like a two-dimension array or a **table** with rows and columns.
    - there are two **axes** (aka **labels**): the **index** (axis 0) and **columns** (axis 1)


#### Series: creation

Creating a `Series` by passing a **list of values**, letting pandas create a default **`RangeIndex`**.

```python
s = pd.Series([1, 3, 5, np.nan, 6, 8]) # index defaults to RangeIndex(0,n)
print(s)
# series properties
print(f"s.index = {s.index}\ns.values = {s.values} \ns.dtype={s.dtype}\ns.shape = {s.shape}\ns.axes = {s.axes}\ns.ndim={s.ndim}")
print(f"s.name={s.name}\ns.size={s.size}\ns.empty={s.empty}\ns.values={s.values}")
```

```python
d = {'a': 1, 'b': 2, 'c': 3, 'd': 4}
ser = pd.Series(data=d, index=['a', 'b', 'c'], name='my_series')
d['a'] = 999 # n.b. changing d doesn't change ser as by default it is copied
print(ser) # note: ('d',4) excluded

arr = np.array([5,7])
ser = pd.Series(arr, index=['b','d'], copy=False) # nb: length of values must match length of index
arr[1] = 999 # n.b. changing arr changes ser as we have overridden the default with copy=False
print(ser)
ser.iloc[0] = 888 # n.b. changing ser changes arr
print(arr)

ser = pd.Series(arr, dtype=np.float64, copy=False)
arr[0] = 0 # n.b.: despite copy=False, the conversion to float implies a copy is done
print(ser)
```

#### DataFrame: creation

The constructor is as follows: `class pandas.DataFrame(data=None, index=None, columns=None, dtype=None, copy=None)`

* you can also pass a dictionary of values, where the **keys** are taken as **columns** and the **values** as the entries for that column (which can be **constants** or **same-sized iterables** with `n` entries, from which size the default `RangeIndex(0,n)` is built)

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


#### Description and basic properties

**`DataFrame.describe()`** shows a quick statistic summary of your data:

```python
df.describe()
```

```python
df.B
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

#### Sorting

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

- Get item: `s[column_label]` os `s[row_slice]`
- Selection by column/index labels: `loc[index_labels,column_labels]` (Access a group of rows/columns by label(s) or a bool array)), `at[row,col]` (access a single value by a row/col pair)
- Selection by position: `iloc[index_slice,col_slice]`, `iat[row_index,col_index]`
- Boolean indexing: e.g. `df[df["A"]>0]`
- Setting values: e.g., `df["F"] = series1`
- Note: While standard Python / NumPy expressions for selecting and setting are intuitive and come in handy for interactive work, for production code, we recommend the optimized pandas data access methods, DataFrame.at(), DataFrame.iat(), DataFrame.loc() and DataFrame.iloc().


#### Data selection via GetItem `[col_label]` or `[row_slice]`

For a `DataFrame`, passing a **single label** as in `df["A"]` **selects a columns and yields a `Series`** (in this case, equivalent to `df.A`):

```python
df["A"] # same as df.A; it selects the  column
```

For a DataFrame, passing a **slice `:` selects matching rows**:


```python
df[0:3] # selects rows
```

#### Data Selection by Label

Using **`DataFrame.loc[index_labels, column_labels]`**

* Allowed inputs: single label (`'a'`); list/array of labels (`['a','b']`); slice object with labels (`'a':'f'`); a Boolean array of the same length as the axis being sliced (`[True,False,True]`); an alignabled Boolean Series/index; a callable function

```python
df.loc[:, :]
```

```python
print(dates, '\n', dates[0])
df.loc[dates[0]]
```

```python
df.loc[:, ["A"]]
```

```python
# label slicing
df.loc["20130102":"20130104", ["A", "B"]]
```

```python
# access to a scalar value
(df.loc[dates[0], "A"], df.at[dates[0], "A"])
```

#### Data Selection by Position: `iloc[]`, `iat[]`

* `iloc[i]`, `iloc[index_slice, column_slice]`, `iloc[list_of_indexes, list_of_columns_positions]`

```python
df.iloc[-1:0:-1, 0:2]
```

```python
df.iloc[3] # 4th row
```

```python
df.iloc[[1,2,4], [0,2]]
```

```python
(df.iloc[1:3, :], df.iloc[:, 1:3])
```

```python
# for getting a value explicitly
(df.iloc[1,1], df.iat[1,1])
```

#### Row and Data Selection by Boolean Indexing

Remember GetItem: `df[column_label]` or `df[row_slice]`

You may **select rows** from a DataFrame using a **boolean vector the same length as the DataFrame's index**

```python
# df[[True] * (len(df.index) // 2)] # Error: length of 3 instead of 6
df[[True] * len(df.index)]
```

```python
print(
    df["A"] > 0.5, 
    '\n\n',
    df[df["A"] > 0.5]
)
```

You may select cells (values) using a Boolean criteron. Notice: this **preserves input data shape** (method **`where`** is used under the hood as the implementation).

```python
print(
    df > 0,
    '\n\n',
    df[df > 0],
    '\n\n',
    df.where(df > 0, other=np.nan) # same as above
)
```

Another method for filtering is **`isin()`** (similar to indexing by a boolean criterion)

```python
import math
df2 = df.copy()
df2["D"] = ["one", "one", "two", "three", "four", "three"]
print(
    df2,
    '\n',
    df2[df2['D'].isin(['two','three'])]
)
```

#### Bulk setting

* **Setting a new column** `df['C'] = series` automatically aligns data by the indexes
* **Setting a cell by labels** `df.at[index_label, col_label] = v`
* **Setting a cell by position** `df.iat[0,1] = 0`
* **Setting multiple cells by condition** `df[df > 0] = v`
* **Setting multiple cells by slicing rows/cols** `df.loc[index_slice, col_slice] = nparr`

```python
sdf = df.copy()
print(sdf)
sdf['A'] = 0
print(sdf)
sdf.at[dates[1], 'A'] = 999
sdf.iat[4, 0] = 888
sdf[sdf < 0] = np.nan
sdf.loc[sdf.index[3:], 'B':'D'] = 55
print(sdf)
```

### Missing data

For NumPy data types, **`np.nan` represents missing data** and it is **by default not included in computations**.

- `df.reindex(index=..., columns=...)` performs **reindexing**, which allows you to change/add/delete the index on a specified axis
- **`df.dropna()`** drops rows that have any missing data (or rows with all missing data if `how=all`)  
- `df.fillna(value=77)` fills missing data
- **`pd.isna(df)`** gets the Boolean mask where values are nan (note: you can't do `df == np.nan` since `np.nan == np.nan` is false, and `df is np.nan` is not what you want)

```python
# reindexing
df0 = df.copy()
print(df0)
df0 = df0.reindex(index=dates[1:4], columns=list(df.columns[1:]) + ['E'])
print(df0)
```

```python
# dropping rows if any/all value is NaN
ddf = df.copy()
ddf.iloc[0,0] = np.nan

ddf.iloc[3] = np.nan
print(ddf)
print(ddf.dropna())
print(ddf.dropna(how='all'))
```

### Operations

Note: Operations **in general exclude missing data**.


#### Stats

```python
df0 = df.copy()
df0[df0<0] = np.nan
print(df0,'\n\n',
      df0.mean(), # implicitly axis=0 (i.e. take the mean along index, returning the mean for each column)
      '\n\n',
      df0.mean(axis=1)
)

```

#### User-def functions: `agg()` and `transform()`

- `agg(f)` applies a user defined function that reduces the df
- `transform(f)` applies a user defined function to produce a new df
- `map(f)` applies a user defined function **element-wise** to produce a new df
- `apply(f)` applies `f` (e.g. numpy universal function `np.sqrt`) element-wise or along a given `axis` (`axis=0` by default) e.g. if `f=np.sum`

```python
# transform, map, apply
df0 = df.reindex(index=dates[2:])
df0[df0<0] = np.nan
print('df=\n', df0)
# nb: with transform(), lambda x: math.floor(x) would not work 
#    since the given function must either work when passed a DataFrame or when passed to DataFrame.apply
print('\ndf.transform(lambda x: x * 10) = \n', df0.transform(lambda x: x * 10)) 
print('\ndf.map(...) = \n', df0.map(lambda x: math.floor(x) if not math.isnan(x) else x))
print('\ndf.apply(np.sqrt) = \n', df0.apply(np.sqrt))
```

#### Value counts

```python
s = pd.Series(np.random.randint(0, 7, size=10))
print(s, '\n', s.value_counts())
```

```python

np.random.seed(5)
df0 = pd.DataFrame(np.random.randint(5, 7, size=15).reshape(5,3), index=list('ABCDE'), columns=list('XYZ')) 
print(df0)
# DataFrame.value_counts(subset=None, normalize=False, sort=True, ascending=False, dropna=True)
# Return a Series containing the frequency of each distinct row in the Dataframe.
df0.value_counts() # with a df, it counts the number of identical rows
```

#### String methods

Series is equipped with a set of string processing methods in the str attribute that make it easy to operate on each element of the array. Cf. Vectorized String Methods.

```python

s = pd.Series(["A", "B", "C", "Aaba", "Baca", np.nan, "CABA", "dog", "cat"])

s.str.lower()
```

### Merging dataframes

- `pd.concat(objs, *, axis=0, join='outer', ...)` concatenate pandas objects along a particular axis.
- `pd.merge(left, right, how='inner', on=None, ...)` merges DataFrame or named Series objects with a database-style join.



#### Concatenating dataframes with `pd.concat(dfs)`

- N.B.: Adding a column to a DataFrame is relatively fast. However, **adding a row requires a copy, and may be expensive**. 
    - RECOMMANDATION: **pass a pre-built list of records to the DataFrame constructor instead of building a DataFrame by iteratively appending records to it**.


```python
df0 = pd.DataFrame(np.random.randn(10, 4))
print(df0)
pieces = [df0[0:2], df0[7:]]
pd.concat(pieces)
```

#### Join: merging by SQL-style join types along specific columns

Types of merges (argument `how='inner'`)

* **inner**: use intersection of keys from both frames, similar to a SQL inner join; preserve the order of the left keys.
* **cross**: creates the cartesian product from both frames, preserves the order of the left keys.
* **left**: use only keys from left frame, similar to a SQL left outer join; preserve key order.
* **right**: use only keys from right frame, similar to a SQL right outer join; preserve key order.
* **outer**: use union of keys from both frames, similar to a SQL full outer join; sort keys lexicographically.

```python
left = pd.DataFrame({"key": ["foo", "foo", "bar", "barl"], "lval": [0, 1, 2, 3]})
right = pd.DataFrame({"key": ["foo", "foo",  "bar", "barr"], "rval": [4, 5, 6, 7]})
print(left, '\n', right)
```

```python
print(
    'inner join = \n', pd.merge(left, right, on="key"), '\n', # default is how='inner' join
    '\nleft join = \n', pd.merge(left, right, how="left"), '\n',    
    '\nouter join = \n', pd.merge(left, right, how="outer"), '\n',
    '\ncross join = \n', pd.merge(left, right, how='cross'), '\n'
)
```

### Grouping

By “group by” we are referring to a process involving one or more of the following steps:

* **Splitting** the data into groups based on some criteria
* **Applying** a function to each group independently
* **Combining** the results into a data structure

```python
np.random.seed(0)
df = pd.DataFrame(
    {
        "A": ["foo", "bar", "foo", "bar", "foo", "bar", "foo", "foo"],
        "B": ["one", "one", "two", "three", "two", "two", "one", "two"],
        "C": np.random.randint(0,9,size=8),
        "D": np.random.randint(0,9,size=8),
    }
)
print(df)
# Grouping by a column label, selecting column labels, and then applying the DataFrameGroupBy.count() function to the resulting groups:
print(df.groupby("A")[["C", "D"]].count())
# Grouping by multiple columns label forms MultiIndex.
print(df.groupby(['B', 'A']).sum())
```

### Reshaping and hierarchical indexing

**Hierarchical / Multi-level indexing** enables you to store and manipulate data with an arbitrary number of dimensions in lower dimensional data structures like Series (1d) and DataFrame (2d).
    - The **`MultiIndex`** object is the hierarchical analogue of the standard Index object which typically stores the axis labels in pandas objects. You can think of MultiIndex as an array of tuples where each tuple is unique.

```python
a = np.array(['foo','foo','bar','bar'])
b = np.array(list('wxyz'))
index = pd.MultiIndex.from_arrays([a,b], names=["first", "second"])
print(index)
hdf = pd.DataFrame(np.random.randint(0, 9, size=8).reshape(4,2), index=index, columns=["A", "B"])
print(hdf)
print('COLUMNS = ', hdf.columns)
```

The **`stack()`** method “compresses” a level in the DataFrame’s columns. I.e. it stacks the prescribed level(s) **from columns to index**. Returns a reshaped DataFrame or Series having a multi-level index with one or more new inner-most levels compared to the current DataFrame. The new inner-most levels are created by pivoting the columns of the current dataframe: if the columns have a single level, the output is a Series; if the columns have multiple levels, the new index level(s) is (are) taken from the prescribed level(s) and the output is a DataFrame.

```python
print(hdf.stack().index)
print(hdf.stack())
print('COLUMNS = ', hdf.stack().columns if hdf.stack().ndim>1 else '(none, i.e., series)')
```

**Unstacking**: Method `unstack()` pivots a level of the (necessarily hierarchical) index labels. Returns a DataFrame having a new level of column labels whose inner-most level consists of the pivoted index labels.

```python
print('hdf.index = ', list(hdf.index), ' with names =', hdf.index.names)
print(hdf)
print('\nhdf.unstack(0).index = ', hdf.unstack(0).index)
print('\nhdf.unstack(0).columns = ', list(hdf.unstack(0).columns))
print(hdf.unstack(0))
print('\nhdf.unstack(1).index = ', hdf.unstack(1).index)
print('\nhdf.unstack(1).columns = ', list(hdf.unstack(1).columns))
print(hdf.unstack(1))

```

These topics are fairly advanced. See [Hierarchical indexing](https://pandas.pydata.org/docs/user_guide/advanced.html#advanced-hierarchical) and [pivot tables](https://pandas.pydata.org/docs/user_guide/reshaping.html#reshaping-pivot) for details.


### Time series

pandas has simple, powerful, and efficient functionality for working with time series

* `Series.resample()` returns a **resampling iterator**, which enables e.g. to aggregate data when performing frequency conversion (e.g., summing data points when converting secondly data into 1-minutely data)
* `Series.tz_localize('UTC')` localizes a time series to a time zone (e.g., 'UTC')
* `Series.tz_convert('US/Eastern')` converts a timezones aware time series to another time zone
* `ts + np.offsets.BusinessDays(5)` adds a non-fixed duration to a time series



```python
rng = pd.date_range("1/1/2012", periods=100, freq="s") # a DateTimeIndex of 100 seconds from 1/1/2012
np.random.seed(0)
ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng) # a Series of data points for the 100 seconds
ts.resample("1Min").sum()
```

### Categoricals

See also [the docs on categoricals](https://pandas.pydata.org/docs/user_guide/categorical.html).

**Categoricals** are a pandas data type corresponding to **categorical variables** in statistics. A categorical variable takes on a **limited, and usually fixed, number of possible values (categories; levels in R)**. 

- Examples are gender, social class, blood type, country affiliation, observation time or rating via Likert scales.
- In contrast to statistical categorical variables, categorical data might have an order (e.g. ‘strongly agree’ vs ‘agree’ or ‘first observation’ vs. ‘second observation’), but numerical operations (additions, divisions, …) are not possible.
- All values of categorical data are either in `categories` or np.nan. Order is defined by the order of categories, not lexical order of the values. Internally, the data structure consists of a categories array and an integer array of codes which point to the real value in the categories array.

Categorical Series or columns in a DataFrame can be created in several ways:

- By specifying **`dtype="category"`** when constructing a Series
- By converting an existing Series or column to a category dtype via `df['A'].astype('category')`
- By using special functions, such as `cut()`, which groups data into discrete bins
- By passing a raw `pandas.Categorical` object to a Series or assigning it to a DataFrame

```python
df = pd.DataFrame(
    {"id": [1, 2, 3, 4, 5, 6], "raw_grade": ["a", "b", "b", "a", "a", "e"]}
)
df["grade"] = df["raw_grade"].astype("category")
print(df)
new_categories = ["very good", "good", "very bad"]
df["grade"] = df["grade"].cat.rename_categories(new_categories)
df = df.sort_values(by="grade")
print(df)
# add more categories (note: missing categories are added)
df["grade"] = df["grade"].cat.set_categories( ["very bad", "bad", "medium", "good", "very good"] )
df.groupby("grade", observed=False).size() # note: by observed=False, also empty categories are observed
```

## Plotting

See [visualization (plotting)](https://pandas.pydata.org/docs/user_guide/visualization.html#visualization) for more.

```python
ts = pd.Series(np.random.randn(1000), index=pd.date_range("1/1/2000", periods=1000))
ts = ts.cumsum()
ts.plot();
```

```python
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index, columns=["A", "B", "C", "D"])
df = df.cumsum()
fig,axes = plt.subplots()
df.plot(ax=axes) # provide an explicit axes
axes.legend(loc='best')

```

### Importing and exporting data

* CSV: `pd.read_csv('path/to/file.csv')` and `DataFrame.to_csv('path/to/file.csv')`
* Similar methods `read_<X>` and `to_<X>` available also for **Parquet** and **Excel**


# Panda: Basics (Inspecting Movies)

The following tutorial introducing the basics of pandas on a movies dataset is based on *Pandas in Action*; see also <https://github.com/paskhaver/pandas-in-action/>.

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
print(m.head(2))
print(m.tail(3))
print(m.to_numpy())
```

```python
print(m.iloc[0], end="\n\n") # first row
print(m2.loc[1], end="\n\n") # by row label 
print(m.sort_values(by = "Year", ascending = True).iloc[0], end="\n\n")
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
