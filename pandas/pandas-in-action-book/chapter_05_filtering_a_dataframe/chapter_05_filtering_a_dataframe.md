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

## 5.1 Optimizing A Data Set for Memory Usage

```python
import pandas as pd
```

```python
pd.read_csv("employees.csv")
```

```python
pd.read_csv("employees.csv", parse_dates = ["Start Date"]).head()
```

```python
employees = pd.read_csv(
    "employees.csv", parse_dates = ["Start Date"]
)
```

```python
employees.info()
```

### 5.1.1 Converting Data Types with the astype Method

```python
employees["Mgmt"].astype(bool)
```

```python
employees["Mgmt"] = employees["Mgmt"].astype(bool)
```

```python
employees.tail()
```

```python
employees.info()
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# employees["Salary"].astype(int)
```

```python
employees["Salary"].fillna(0).tail()
```

```python
employees["Salary"].fillna(0).astype(int).tail()
```

```python
employees["Salary"] = employees["Salary"].fillna(0).astype(int)
```

```python
employees.nunique()
```

```python
employees["Gender"].astype("category")
```

```python
employees["Gender"] = employees["Gender"].astype("category")
```

```python
employees.info()
```

```python
employees["Team"] = employees["Team"].astype("category")
```

```python
employees.info()
```

## 5.2 Filtering by a Single Condition

```python
"Maria" == "Maria"
```

```python
"Maria" == "Taylor"
```

```python
employees["First Name"] == "Maria"
```

```python
employees[employees["First Name"] == "Maria"]
```

```python
marias = employees["First Name"] == "Maria"
employees[marias]
```

```python
"Finance" != "Engineering"
```

```python
employees["Team"] != "Finance"
```

```python
employees[employees["Team"] != "Finance"]
```

```python
employees[employees["Mgmt"]].head()
```

```python
high_earners = employees["Salary"] > 100000
high_earners.head()
```

```python
employees[high_earners].head()
```

## 5.3 Filtering by Multiple Conditions


### 5.3.1 The AND Condition

```python
is_female = employees["Gender"] == "Female"
```

```python
in_biz_dev = employees["Team"] == "Business Dev"
```

```python
employees[is_female & in_biz_dev].head()
```

```python
is_manager = employees["Mgmt"]
employees[is_female & in_biz_dev & is_manager].head()
```

### 5.3.2 The OR Condition

```python
earning_below_40k = employees["Salary"] < 40000
started_after_2015 = employees["Start Date"] > "2015-01-01"
```

```python
employees[earning_below_40k | started_after_2015].tail()
```

### 5.3.3 Inversion with ~

```python
my_series = pd.Series([True, False, True])
my_series
```

```python
~my_series
```

```python
employees[employees["Salary"] < 100000].head()
```

```python
employees[~(employees["Salary"] >= 100000)].head()
```

### 5.3.4 Methods for Booleans


## 5.4 Filtering by Condition


### 5.4.1 The isin Method

```python
sales = employees["Team"] == "Sales"
legal = employees["Team"] == "Legal"
mktg  = employees["Team"] == "Marketing"
employees[sales | legal | mktg].head()
```

```python
all_star_teams = ["Sales", "Legal", "Marketing"]
on_all_star_teams = employees["Team"].isin(all_star_teams)
employees[on_all_star_teams].head()
```

### 5.4.2 The between Method

```python
higher_than_80 = employees["Salary"] >= 80000
lower_than_90 = employees["Salary"] < 90000
employees[higher_than_80 & lower_than_90].head()
```

```python
between_80k_and_90k = employees["Salary"].between(80000, 90000)
employees[between_80k_and_90k].head()
```

```python
eighties_folk = employees["Start Date"].between(
    left = "1980-01-01",
    right = "1990-01-01"
)

employees[eighties_folk].head()
```

```python
name_starts_with_r = employees["First Name"].between("R", "S")
employees[name_starts_with_r].head()
```

### 5.4.3 The isnull and notnull Methods

```python
employees.head()
```

```python
employees["Team"].isnull().head()
```

```python
employees["Start Date"].isnull().head()
```

```python
employees["Team"].notnull().head()
```

```python
(~employees["Team"].isnull()).head()
```

```python
no_team = employees["Team"].isnull()
employees[no_team].head()
```

```python
has_name = employees["First Name"].notnull()
employees[has_name].tail()
```

### 5.4.4 Dealing with Null Values

```python
employees = pd.read_csv(
    "employees.csv", parse_dates = ["Start Date"]
)
```

```python
employees
```

```python
employees.dropna()
```

```python
employees.dropna(how = "all").tail()
```

```python
employees.dropna(how = "any").tail()
```

```python
employees.dropna(subset = ["Gender"]).tail()
```

```python
employees.dropna(subset = ["Start Date", "Salary"]).head()
```

```python
employees.dropna(how = "any", thresh = 4).head()
```

## 5.5 Dealing with Duplicates


### 5.5.1 The duplicated Method

```python
employees["Team"].head()
```

```python
employees["Team"].duplicated().head()
```

```python
employees["Team"].duplicated(keep = "first").head()
```

```python
employees["Team"].duplicated(keep = "last")
```

```python
(~employees["Team"].duplicated()).head()
```

```python
first_one_in_team = ~employees["Team"].duplicated()
employees[first_one_in_team]
```

### 5.5.2 The drop_duplicates Method

```python
employees.drop_duplicates()
```

```python
employees.drop_duplicates(subset = ["Team"])
```

```python
employees.drop_duplicates(subset = ["Team"], keep = "last")
```

```python
employees.drop_duplicates(subset = ["First Name"], keep = False)
```

```python
name_is_douglas = employees["First Name"] == "Douglas"
is_male = employees["Gender"] == "Male"
employees[name_is_douglas & is_male]
```

```python
employees.drop_duplicates(subset = ["Gender", "Team"]).head()
```

## 5.6 Coding Challenge


### 5.6.1 Problems


### 5.6.2 Solutions

```python
pd.read_csv("netflix.csv")
```

```python
netflix = pd.read_csv("netflix.csv", parse_dates = ["date_added"])
```

```python
netflix.info()
```

```python
netflix.nunique()
```

```python
netflix["type"] = netflix["type"].astype("category")
```

```python
netflix.info()
```

```python
netflix[netflix["title"] == "Limitless"]
```

```python
directed_by_robert_rodriguez = (
    netflix["director"] == "Robert Rodriguez"
)
is_movie = netflix["type"] == "Movie"
netflix[directed_by_robert_rodriguez & is_movie]
```

```python
added_on_july_31 = netflix["date_added"] == "2019-07-31"
directed_by_altman = netflix["director"] == "Robert Altman"
netflix[added_on_july_31 | directed_by_altman]
```

```python
directors = ["Orson Welles", "Aditya Kripalani", "Sam Raimi"]
target_directors = netflix["director"].isin(directors)
netflix[target_directors]
```

```python
may_movies = netflix["date_added"].between(
    "2019-05-01", "2019-06-01"
)

netflix[may_movies].head()
```

```python
netflix.dropna(subset = ["director"]).head()
```

```python
netflix.drop_duplicates(subset = ["date_added"], keep = False)
```

## 5.7 Summary

```python

```
