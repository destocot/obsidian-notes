---
title: "Python Basics 3"
sidebar:
  order: 3
---

## booleans

```py
is_authenticated = True

print(is_authenticated) # True
print(not is_authenticated) # False
```

### comparison operators

```py
x, y = 5, 10

print(x > y) # False
print(x < y) # True
print(x == y) # False
print(x != y) # True
print (x >= y) # False
print(x <= y) # True
```

### boolean operators & member checking

```py
x, y = True, False

print(x and y) # False
print(x or y) # True
```

### falsy values --> 0 numbers and empty data structs

```py
print(bool(0)) # False
print(bool("")) # False
print(bool([])) # False
```

### truthy values --> everything else

```py
print(bool(1)) # True
print(bool("hello, ninjas")) # True
print(bool([100, 99, 98])) # True
```

### evaluating strings

```py
name = "mario"

print(name.startswith("m")) # True
print(name.startswith("a")) # False
print(name.endswith("o")) # True
```

## Sets

Sets are unordered & unique elements

```py
ingredients = { "tofu", "avocado", "cherries", "pasta", "tofu" }
print(ingredients)  # Duplicates ("tofu") are automatically removed
```

### make a set using the set function around a list

```py
scores = set([100, 25, 37, 100, 27])  # Converts a list to a set, removing duplicates
print(scores)
```

### add and remove methods

```py
ingredients.add("tomato")         # Adds an element to the set
ingredients.remove("cherries")    # Removes an element; raises KeyError if not found

# ingredients.remove("apple") # Would raise KeyError since "apple" isn't in the set
ingredients.discard("apple")      # Removes element if found; no error if not found

print(ingredients)
```

### joining sets (union)

```py
set_one = { 1, 2, 3 }
set_two = { 3, 4, 5 }

union_set = set_one.union(set_two)  # Combines elements from both sets
print(union_set)
```

### intersection (set of common elements)

```py
int_set = set_one.intersection(set_two)  # Elements common to both sets
print(int_set)
```

### frozen (immutable) sets

```py
aages = frozenset([19, 21, 35, 25])
print(ages)  # Immutable set, cannot be modified

# ages.add(10) # Would raise AttributeError because frozensets don't support adding elements
```

## Formatted Strings (f-strings)

```py
name = 'mario'
score = 50
```

### examples - newer

```py
print(f'{name} has a score of {score}')
# mario has a score of 50
```

### older approach

```py
print("{} has a score of {}".format(name, score))
# mario has a score of 50

print("{1} has a score of {0}".format(name, score))
# 50 has a score of mario

print("{n} has a score of {s}".format(n=name, s=score))
# mario has a score of 50
```

### expressions in f-strings

```py
print(f'{name.capitalize()} has a bonus score of {score * 2}')
```

## User Input

```py
name = input('Your name, please: ')
# Prompts the user to enter their name as a string
age = int(input("Your age, please: "))
# Prompts the user to enter their age, converted to an integer

print(f'Hello {name}, you are {age} years old')
# Outputs a formatted string with the user's name and age

a = float(input("First number: "))
# Prompts the user to enter a number, converted to a float

b = float(input("Second number: "))
# Prompts the user to enter another number, also converted to a float

print(f'The sum of those 2 numbers is { a + b }')
# Outputs the sum of the two numbers
```
