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

## C.1 Dimensions


## C.2 The ndarray Object

```python
import numpy as np
```

### C.2.1. Generating a Numeric Range with the arange Method

```python
np.arange(3)
```

```python
np.arange(2, 6)
```

```python
np.arange(start = 2, stop = 6)
```

```python
np.arange(start = 0, stop = 111, step = 10)
```

```python
tens = np.arange(start = 0, stop = 111, step = 10)
```

### C.2.2 Attributes on a ndarray Object

```python
tens.shape
```

```python
tens.ndim
```

```python
tens.size
```

### C.2.3 The reshape Method

```python
tens
```

```python
tens.reshape(4, 3)
```

```python
tens.reshape(4, 3).ndim
```

```python
tens.reshape(2, 6)
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# tens.reshape(2, 5)
```

```python
tens.reshape(2, 3, 2)
```

```python
tens.reshape(2, 3, 2).ndim
```

```python
tens.reshape(2, -1)
```

```python
tens.reshape(2, -1, 2)
```

### C.2.4 The randint Function

```python
np.random.randint(5)
```

```python
np.random.randint(1, 10)
```

```python
np.random.randint(1, 10, 3)
```

```python
np.random.randint(1, 10, [3])
```

```python
np.random.randint(1, 10, [3, 5])
```

### C.2.5 The randn Function

```python
np.random.randn(3)
```

```python
np.random.randn(2, 4)
```

```python
np.random.randn(2, 4, 3)
```

## C.3 The nan Object

```python
np.nan
```

```python
np.nan == 5
```

```python
np.nan == np.nan
```

## C.4 Summary

```python

```
