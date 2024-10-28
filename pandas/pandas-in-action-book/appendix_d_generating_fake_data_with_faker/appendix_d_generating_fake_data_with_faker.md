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

## D.1 Installation


## D.2 Getting Started with Faker

```python
import pandas as pd
import numpy as np
import faker 
```

```python
fake = faker.Faker()
```

```python
fake.name()
```

```python
fake.name_male()
```

```python
fake.name_female()
```

```python
fake.first_name()
```

```python
fake.last_name()
```

```python
fake.first_name_male()
```

```python
fake.first_name_female()
```

```python
fake.address()
```

```python
print(fake.address())
```

```python
fake.street_address()
```

```python
fake.city()
```

```python
fake.state()
```

```python
fake.postcode()
```

```python
fake.company()
```

```python
fake.catch_phrase()
```

```python
fake.job()
```

```python
fake.url()
```

```python
fake.email()
```

```python
fake.phone_number()
```

```python
fake.credit_card_number()
```

## D.3 Populating a DataFrame with Fake Values

```python
for i in range(5):
    print(i)
```

```python
first_name = fake.first_name_female()
last_name = fake.last_name()
email = first_name + last_name + "@gmail.com"
email
```

```python
data = [
    { "Name": fake.name(), 
      "Company": fake.company(), 
      "Email": fake.email(), 
      "Salary": np.random.randint(50000, 200000) 
    }
    for i in range(1000)
]
```

```python
df = pd.DataFrame(data = data)
df
```

## D.4 Summary

```python

```
