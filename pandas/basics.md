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

```python
import pandas as pd
m = pd.read_csv("../data/movies.csv")
print(type(m))
m
```

Work with a dataframe
