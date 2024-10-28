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

## 2.1 Overview of a Series

```python
import pandas as pd
import numpy as np
```

### 2.1.1 Classes, and Instances

```python
pd.Series()
```

### 2.1.2 Populating the Series with Values

```python
ice_cream_flavors = [
    "Chocolate",
    "Vanilla",
    "Strawberry",
    "Rum Raisin",
]

pd.Series(ice_cream_flavors)
```

```python
# The two lines below are equivalent
pd.Series(ice_cream_flavors)
pd.Series(data = ice_cream_flavors)
```

### 2.1.3 Customizing the Series Index

```python
ice_cream_flavors = [
    "Chocolate",
    "Vanilla",
    "Strawberry",
    "Rum Raisin",
]

days_of_week = ("Monday", "Wednesday", "Friday", "Saturday")

# The two lines below are equivalent
pd.Series(ice_cream_flavors, days_of_week)
pd.Series(data = ice_cream_flavors, index = days_of_week)
```

```python
ice_cream_flavors = [
    "Chocolate",
    "Vanilla",
    "Strawberry",
    "Rum Raisin",
]

days_of_week = ("Monday", "Wednesday", "Friday", "Wednesday")

# The two lines below are equivalent
pd.Series(ice_cream_flavors, days_of_week)
pd.Series(data = ice_cream_flavors, index = days_of_week)
```

```python
pd.Series(index = days_of_week, data = ice_cream_flavors)
```

```python
bunch_of_bools = [True, False, False]
pd.Series(bunch_of_bools)
```

```python
stock_prices = [985.32, 950.44]
time_of_day = ["Open", "Close"]
pd.Series(data = stock_prices, index = time_of_day)
```

```python
lucky_numbers = [4, 8, 15, 16, 23, 42]
pd.Series(lucky_numbers)
```

```python
lucky_numbers = [4, 8, 15, 16, 23, 42]
pd.Series(lucky_numbers, dtype = "float")
```

### 2.1.4 Creating a Series with Missing Values

```python
temperatures = [94, 88, np.nan, 91]
pd.Series(data = temperatures)
```

## 2.2 Create a Series from Python Objects

```python
calorie_info = {
    "Cereal": 125,
    "Chocolate Bar": 406,
    "Ice Cream Sundae": 342,
}

diet = pd.Series(calorie_info)
diet
```

```python
pd.Series(data = ("Red", "Green", "Blue"))
```

```python
rgb_colors = [(120, 41, 26), (196, 165, 45)]
pd.Series(data = rgb_colors)
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
my_set = {"Ricky", "Bobby"}
# pd.Series(my_set)
```

```python
pd.Series(list(my_set))
```

```python
random_data = np.random.randint(1, 101, 10)
random_data
```

```python
pd.Series(random_data)
```

## 2.3 Series Attributes

```python
diet.values
```

```python
type(diet.values)
```

```python
diet.index
```

```python
type(diet.index)
```

```python
diet.dtype
```

```python
diet.size
```

```python
diet.shape
```

```python
diet.is_unique
```

```python
pd.Series(data = [3, 3]).is_unique
```

```python
pd.Series(data = [1, 3, 6]).is_monotonic
```

```python
pd.Series(data = [1, 6, 3]).is_monotonic
```

## 2.4 Retrieving the First and Last Rows

```python
values = range(0, 500, 5)
nums = pd.Series(data = values)
nums
```

```python
nums.head(3)
```

```python
nums.head(n = 3)
```

```python
nums.head()
```

```python
nums.tail(6)
```

```python
nums.tail()
```

## 2.5 Mathematical Operations


### 2.5.1 Statistical Operations

```python
numbers = pd.Series([1, 2, 3, np.nan, 4, 5])
numbers
```

```python
numbers.count()
```

```python
numbers.sum()
```

```python
numbers.sum(skipna = False)
```

```python
numbers.sum(min_count = 3)
```

```python
numbers.sum(min_count = 6)
```

```python
numbers.product()
```

```python
numbers.product(skipna = False)
```

```python
numbers.product(min_count = 3)
```

```python
numbers
```

```python
numbers.cumsum()
```

```python
numbers.cumsum(skipna = False)
```

```python
numbers
```

```python
numbers.pct_change()
```

```python
# The three lines below are equivalent
numbers.pct_change()
numbers.pct_change(fill_method = "pad")
numbers.pct_change(fill_method = "ffill")
```

```python
# The two lines below are equivalent
numbers.pct_change(fill_method = "bfill")
numbers.pct_change(fill_method = "backfill")
```

```python
numbers.mean()
```

```python
numbers.median()
```

```python
numbers.std()
```

```python
numbers.max()
```

```python
numbers.min()
```

```python
animals = pd.Series(["koala", "aardvark", "zebra"])
animals
```

```python
animals.max()
```

```python
animals.min()
```

```python
numbers.describe()
```

```python
numbers.sample(3)
```

```python
authors = pd.Series(
    ["Hemingway", "Orwell", "Dostoevsky", "Fitzgerald", "Orwell"]
)

authors.unique()
```

```python
authors.nunique()
```

### 2.5.2 Arithmetic Operations

```python
s1 = pd.Series(data = [5, np.nan, 15], index = ["A", "B", "C"])
s1
```

```python
s1 + 3
```

```python
s1.add(3)
```

```python
# The three lines below are equivalent
s1 - 5
s1.sub(5)
s1.subtract(5)
```

```python
# The three lines below are equivalent
s1 * 2
s1.mul(2)
s1.multiply(2)
```

```python
# The three lines below are equivalent
s1 / 2
s1.div(2)
s1.divide(2)
```

```python
# The two lines below are equivalent
s1 // 4
s1.floordiv(4)
```

```python
# The two lines below are equivalent
s1 % 3
s1.mod(3)
```

### 2.5.3 Broadcasting

```python
s1 = pd.Series([1, 2, 3], index = ["A", "B", "C"])
s2 = pd.Series([4, 5, 6], index = ["A", "B", "C"])
```

```python
s1 + s2
```

```python
s1 = pd.Series(data = [3, 6, np.nan, 12])
s2 = pd.Series(data = [2, 6, np.nan, 12])
```

```python
# The two lines below are equivalent
s1 == s2
s1.eq(s2)
```

```python
# The two lines below are equivalent
s1 != s2
s1.ne(s2)
```

```python
s1 = pd.Series(
    data = [5, 10, 15], index = ["A", "B", "C"]
)

s2 = pd.Series(
    data = [4, 8, 12, 14], index = ["B", "C", "D", "E"]
)
```

```python
s1 + s2
```

## 2.6 Passing the Series to Python's Built-In Functions

```python
cities = pd.Series(
    data = ["San Francisco", "Los Angeles", "Las  Vegas", np.nan]
)
```

```python
len(cities)
```

```python
type(cities)
```

```python
dir(cities)
```

```python
list(cities)
```

```python
dict(cities)
```

```python
cities
```

```python
"Las Vegas" in cities
```

```python
2 in cities
```

```python
"Las Vegas" in cities.values
```

```python
100 not in cities
```

```python
"Paris" not in cities.values
```

## 2.7 Coding Challenge


### 2.7.1 Problems


### 2.7.2 Solutions

```python
superheroes = [
    "Batman",
    "Superman",
    "Spider-Man",
    "Iron Man",
    "Captain America",
    "Wonder Woman"
]
```

```python
strength_levels = (100, 120, 90, 95, 110, 120)
```

```python
pd.Series(superheroes)
```

```python
pd.Series(data = strength_levels)
```

```python
heroes = pd.Series(
    data = strength_levels, index = superheroes
)

heroes
```

```python
heroes.head(2)
```

```python
heroes.tail(4)
```

```python
heroes.nunique()
```

```python
heroes.mean()
```

```python
heroes.max()
```

```python
heroes.min()
```

```python
heroes * 2
```

```python
dict(heroes)
```

## 2.7 Summary

```python

```
