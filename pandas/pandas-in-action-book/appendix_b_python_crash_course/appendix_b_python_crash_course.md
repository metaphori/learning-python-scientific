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

## B.1 Simple Data Types


### B.1.1 Numbers

```python
20
```

```python
-13
```

```python
7.349
```

### B.1.2 Strings

```python
'Good morning'
```

```python
"Good afternoon"
```

```python
"""Good night"""
```

```python
"$15 dollars!"
```

```python
""
```

```python
"Python"[3]
```

```python
"Python"[-4]
```

```python
"Python"[2:5]
```

```python
# The two lines below are equivalent
"Python"[0:4]
"Python"[:4]
```

```python
# The two lines below are equivalent
"Python"[3:6]
"Python"[3:]
```

```python
"Python"[:]
```

```python
"Python"[1:-1]
```

```python
"Python"[0:6:2]
```

```python
"Python"[::-1]
```

### B.1.3  Booleans

```python
True
```

```python
False
```

### B.1.4 The None Object

```python
None
```

## B.2 Operators


### B.2.1 Mathematical Operators

```python
3 + 5
```

```python
3 - 5
```

```python
3 * 5
```

```python
3 ** 5
```

```python
3 / 5
```

```python
18 / 6
```

```python
8 / 3
```

```python
8 // 3
```

```python
5 % 3
```

```python
"race" + "car"
```

```python
"Mahi" * 2
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# 3 + "5"
```

### B.2.2 Equality and Inequality Operators

```python
10 == 10
```

```python
10 == 20
```

```python
"Hello" == "Hello"
```

```python
"Hello" == "Goodbye"
```

```python
"Hello" == "hello"
```

```python
10 != 20
```

```python
"Hello" != "Goodbye"
```

```python
10 != 10
```

```python
"Hello" != "Hello"
```

```python
-5 < 3
```

```python
5 > 7
```

```python
11 <= 11
```

```python
4 >= 5
```

## B.3 Variables

```python
name = "Boris"
age = 28
high_school_gpa = 3.7
is_handsome = True
```

```python
name
```

```python
age = 35
age
```

```python
age = age + 10
age
```

```python
high_school_gpa = "A+"
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# last_name
```

## B.4 Functions


### B.4.1 Arguments and Return Values

```python
len("Python is fun")
```

```python
int("20")
```

```python
float("14.3")
```

```python
str(5)
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# int("xyz")
```

```python
value = 10
print(value)

value = value - 3
print(value)

value = value * 4
print(value)

value = value / 2
print(value)
```

```python
print("Cherry", "Strawberry", "Key Lime")
```

```python
print("Cherry", "Strawberry", "Key Lime", sep = "!")
```

```python
# The two lines below are equivalent
print("Cherry", "Strawberry", "Key Lime")
print("Cherry", "Strawberry", "Key Lime", sep=" ")
```

```python
print("Cherry", "Strawberry", "Key Lime", sep="*!*")
```

```python
print("Cherry", "Strawberry", "Key Lime", sep="!", end="\n")
print("Peach Cobbler")
```

```python
print("Cherry", "Strawberry", "Key Lime", sep="!", end="***")
print("Peach Cobbler")
```

```python
print(
    "Cherry", "Strawberry", "Key Lime", sep="!", end="***"
)
```

```python
print(
    "Cherry",
    "Strawberry",
    "Key Lime",
    sep="!",
    end="***",
)
```

### B.4.2 Custom Functions

```python
def convert_to_fahrenheit(celsius_temp):
    first_step = celsius_temp * (9 / 5)
    fahrenheit_temperature = first_step + 32
    return fahrenheit_temperature
```

```python
convert_to_fahrenheit(10)
```

```python
convert_to_fahrenheit(celsius_temp = 10)
```

## B.5 Modules

```python
import datetime
```

```python
import datetime as dt
```

## B.6 Classes and Objects

```python
type("peanut butter")
```

```python
type("jelly")
```

```python
da_vinci_birthday = dt.date(1452, 4, 15)
da_vinci_birthday
```

### B.6.1 Attributes and Methods

```python
da_vinci_birthday.day
```

```python
da_vinci_birthday.month
```

```python
da_vinci_birthday.year
```

```python
da_vinci_birthday.weekday()
```

## B.7 String Methods

```python
"Hello".upper()
```

```python
greeting = "Hello"
greeting.upper()
```

```python
greeting
```

```python
"1611 BROADWAY".lower()
```

```python
"uPsIdE dOwN".swapcase()
```

```python
"Sally Sells Seashells by the Seashore".replace("S", "$")
```

```python
"  ".isspace()
```

```python
"3 Amigos".isspace()
```

```python
data = "    10/31/2019  "
data.rstrip()
```

```python
data.lstrip()
```

```python
data.strip()
```

```python
"robert".capitalize()
```

```python
"once upon a time".title()
```

```python
"BENJAMIN FRANKLIN".lower().title()
```

```python
"tuna" in "fortunate"
```

```python
"salmon" in "fortunate"
```

```python
"factory".startswith("fact")
```

```python
"garage".endswith("rage")
```

```python
"celebrate".count("e")
```

```python
"celebrate".find("e")
```

```python
"celebrate".index("e")
```

```python
"celebrate".find("z")
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# "celebrate".index("z")
```

## B.8 Lists

```python
backstreet_boys = ["Nick", "AJ", "Brian", "Howie", "Kevin"]
```

```python
len(backstreet_boys)
```

```python
[]
```

```python
prime_numbers = [2, 3, 5, 7, 11]
```

```python
stock_prices_for_last_four_days = [99.93, 105.23, 102.18, 94.45]
```

```python
settings = [True, False, False, True, True, False]
```

```python
motley_crew = ["rhinoceros", 42, False, 100.05]
```

```python
favorite_foods = ["Sushi", "Steak", "Barbeque"]
```

```python
favorite_foods = ["Sushi", "Steak", "Barbeque",]
```

```python
favorite_foods = [
    "Sushi",
    "Steak",
    "Barbeque",
]
```

```python
favorite_foods[1]
```

```python
favorite_foods[1:3]
```

```python
favorite_foods[:2]
```

```python
favorite_foods[2:]
```

```python
favorite_foods[:]
```

```python
favorite_foods[0:3:2]
```

```python
favorite_foods.append("Burrito")
favorite_foods
```

```python
favorite_foods.extend(["Tacos", "Pizza", "Cheeseburger"])
favorite_foods
```

```python
favorite_foods.insert(2, "Pasta")
favorite_foods
```

```python
"Pizza" in favorite_foods
```

```python
"Caviar" in favorite_foods
```

```python
"Pizza" not in favorite_foods
```

```python
"Caviar" not in favorite_foods
```

```python
favorite_foods.append("Pasta")
favorite_foods
```

```python
favorite_foods.count("Pasta")
```

```python
favorite_foods.remove("Pasta")
favorite_foods
```

```python
favorite_foods.pop()
```

```python
favorite_foods
```

```python
favorite_foods.pop(2)
```

```python
favorite_foods
```

```python
spreadsheet = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
```

### B.8.1 List Iteration

```python
for season in ["Winter", "Spring", "Summer", "Fall"]:
    print(len(season))
```

```python
letter_count = 0

for season in ["Winter", "Spring", "Summer", "Fall"]:
    letter_count = letter_count + len(season)

letter_count
```

### B.8.2 List Comprehension

```python
numbers = [4, 8, 15, 16, 23, 42]
```

```python
squares = []

for number in numbers:
    squares.append(number ** 2)

squares
```

```python
squares = [number ** 2 for number in numbers]
squares
```

### B.8.3. Converting a String to a List and Vice Versa

```python
empire_state_bldg = "20 West 34th Street, New York, NY, 10001"
```

```python
empire_state_bldg.split(",")
```

```python
empire_state_bldg.split(", ")
```

```python
chrysler_bldg = ["405 Lexington Ave", "New York", "NY", "10174"]
```

```python
", ".join(chrysler_bldg)
```

## B.9 Tuples

```python
"Rock", "Pop", "Country"
```

```python
music_genres = ("Rock", "Pop", "Country")
music_genres
```

```python
len(music_genres)
```

```python
one_hit_wonders = ("Never Gonna Give You Up")
one_hit_wonders
```

```python
one_hit_wonders = ("Never Gonna Give You Up", )
one_hit_wonders
```

```python
empty_tuple = tuple()
empty_tuple
```

```python
len(empty_tuple)
```

## B.10 Dictionaries

```python
{}
```

```python
{ "Cheeseburger": 7.99 }
```

```python
menu = {"Cheeseburger": 7.99, "French Fries": 2.99, "Soda": 2.99}
menu
```

```python
len(menu)
```

```python
menu["French Fries"]
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# menu["Steak"]
```

```python
# menu["soda"]
```

```python
menu.get("French Fries")
```

```python
print(menu.get("Steak"))
```

```python
menu.get("Steak", 99.99)
```

```python
menu["Taco"] = 0.99
menu
```

```python
print(menu["Cheeseburger"])
menu["Cheeseburger"] = 9.99
print(menu["Cheeseburger"])
```

```python
menu.pop("French Fries")
```

```python
menu
```

```python
"Soda" in menu
```

```python
"Spaghetti" in menu
```

```python
1.99 in menu.values()
```

```python
499.99 in menu.values()
```

### B.10.1 Dictionary Iteration

```python
capitals = {
    "New York": "Albany",
    "Florida": "Tallahassee",
    "California": "Sacramento"
}

for state, capital in capitals.items():
    print("The capital of " + state + " is " + capital + ".")
```

## B.11 Sets

```python
favorite_numbers = { 4, 8, 15, 16, 23, 42 }
```

```python
set()
```

```python
favorite_numbers.add(100)
favorite_numbers
```

```python
favorite_numbers.add(15)
favorite_numbers
```

**NOTE**: I've commented out the code below so that the Notebook can run without raising an error.

```python
# favorite_numbers[2]
```

### B.11.1 Set Operations

```python
candy_bars = { "Milky Way", "Snickers", "100 Grand" }
sweet_things = { "Sour Patch Kids", "Reeses Pieces", "Snickers" }
```

```python
candy_bars.intersection(sweet_things)
```

```python
candy_bars & sweet_things
```

```python
candy_bars.union(sweet_things)
```

```python
candy_bars | sweet_things
```

```python
candy_bars.difference(sweet_things)
```

```python
candy_bars - sweet_things
```

```python
candy_bars.symmetric_difference(sweet_things)
```

```python
candy_bars ^ sweet_things
```

## B.12 Summary

```python

```
