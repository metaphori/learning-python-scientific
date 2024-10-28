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

## 13.1 Getting and Setting pandas Options

```python
import pandas as pd
```

```python
happiness = pd.read_csv("happiness.csv")
happiness.head()
```

```python
pd.describe_option("display.max_rows")
```

```python
pd.describe_option("max_col")
```

```python
# The two lines below are equivalent
pd.get_option("display.max_rows")
pd.options.display.max_rows
```

```python
# The two lines below are equivalent
pd.set_option("display.max_rows", 6)
pd.options.display.max_rows = 6
```

```python
pd.options.display.max_rows
```

```python
happiness.head(6)
```

```python
happiness.head(7)
```

```python
# The two lines below are equivalent
pd.get_option("display.max_columns")
pd.options.display.max_columns
```

```python
# The two lines below are equivalent
pd.set_option("display.max_columns", 2)
pd.options.display.max_columns = 2
```

```python
happiness.head(7)
```

```python
# The two lines below are equivalent
pd.set_option("display.max_columns", 5)
pd.options.display.max_columns = 5
```

```python
happiness.head()
```

```python
pd.reset_option("display.max_rows")
```

```python
pd.get_option("display.max_rows")
```

## 13.2 Precision

```python
pd.describe_option("display.precision")
```

```python
# The two lines below are equivalent
pd.set_option("display.precision", 2)
pd.options.display.precision = 2
```

```python
happiness.head()
```

```python
happiness.loc[0, "Score"]
```

## 13.3 Maximum Column Width

```python
pd.describe_option("display.max_colwidth")
```

```python
# The two lines below are equivalent
pd.set_option("display.max_colwidth", 9)
pd.options.display.max_colwidth = 9
```

```python
happiness.tail()
```

## 13.4 Chop Threshold

```python
pd.describe_option("display.chop_threshold") 
```

```python
pd.set_option("display.chop_threshold", 0.25)
```

```python
happiness.tail()
```

## 13.5 Option Context

```python
with pd.option_context(
    "display.max_columns", 5, 
    "display.max_rows", 10, 
    "display.precision", 3
):
    display(happiness)
```

## 13.6 Summary

```python

```
