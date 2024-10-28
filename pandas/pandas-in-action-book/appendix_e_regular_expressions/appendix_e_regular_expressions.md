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

## E.1 An Introduction to Python's re Module

```python
import re
import pandas as pd
```

```python
re.search("flower", "field of flowers")
```

```python
print(re.search("flower", "Barney the Dinosaur"))
```

```python
re.findall("flower", "Picking flowers in the flower field")
```

## E.2 Metacharacters

```python
sentence = "I went to the store and bought " \
           "5 apples, 4 oranges, and 15 plums."

sentence
```

```python
re.findall("\d", sentence)
```

```python
print(re.findall(r"\d", sentence))
```

```python
print(re.findall(r"\D", sentence))
```

```python
print(re.findall(r"\w", sentence))
```

```python
print(re.findall(r"\W", sentence))
```

```python
print(re.findall(r"\s", sentence))
```

```python
print(re.findall(r"\S", sentence))
```

```python
print(re.findall(r"t", sentence))
```

```python
print(re.findall(r"to", sentence))
```

```python
print(re.findall(r"\bt", sentence))
```

```python
print(re.findall(r"t\b", sentence))
```

```python
print(re.findall(r"\Bt", sentence))
```

## E.3 Advanced Search Patterns

```python
soda = "coca cola."
soda
```

```python
print(re.findall(r".", soda))
```

```python
print(re.findall(r"c.", soda))
```

```python
print(re.findall(r"\.", soda))
```

```python
print(re.findall(r"co", soda))
```

```python
print(re.findall(r"[co]", soda))
```

```python
print(re.findall(r"[oc]", soda))
```

```python
print(re.findall(r"[cdefghijkl]", soda))
```

```python
print(re.findall(r"[c-l]", soda))
```

```python
word = "bookkeeper"
word
```

```python
print(re.findall(r"ee", word))
```

```python
print(re.findall(r"e{2}", word))
```

```python
print(re.findall(r"e{3}", word))
```

```python
print(re.findall(r"e{1,3}", word))
```

```python
transcription = "I can be reached at 555-123-4567. "\
                "Look forward to talking to you soon."

transcription
```

```python
print(re.findall(r"\d{3}-\d{3}-\d{4}", transcription))
```

```python
print(re.findall(r"\d+-\d+-\d+", transcription))
```

## E.4 Regular Expressions and pandas

```python
ice_cream = pd.read_csv("ice_cream.csv")
ice_cream.head()
```

```python
ice_cream["Description"].str.extract(r"(\bChocolate\s\w+)").head()
```

```python
(
    ice_cream["Description"]
    .str.extract(r"(\bChocolate\s\w+)")
    .dropna()
    .head()
)
```

```python
(
    ice_cream["Description"]
    .str.extract(r"(\bChocolate\s\w+)")
    .dropna()
    .squeeze()
    .head()
)
```

```python
chocolate_flavors = (
    ice_cream["Description"]
    .str.extract(r"(\bChocolate\s\w+)")
    .dropna()
    .squeeze()
)
```

```python
chocolate_flavors.str.split(r"\s").head()
```

```python
chocolate_flavors.str.split(r"\s").str.get(1).head()
```

```python
chocolate_flavors.str.split(r"\s").str.get(1).value_counts()
```

## E.5 Summary

```python

```
