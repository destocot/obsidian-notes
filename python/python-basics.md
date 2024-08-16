---
title: "Python Basics"
sidebar:
  order: 1
---

## Introduction

Python is

- a **general purpose** programming language
- a **dynamically typed** language
- a **strongly typed** language

## Installing Python 3

```bash
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt update
sudo apt install python3.12
```

### Uninstall

```bash
sudo apt remove --autoremove python3.12
sudo add-apt-repository --remove ppa:deadsnakes/ppa
```

## strings, numbers, and comments

### Variables

```py
name = 'mario'
age = 30
height = 5.4

name = 'luigi'
age = 32
height = 'five ft. four in.'
```

### Printing

```py
print(name, age, height)
```

### Type Errors

```py
print(name + " is " + height)

# print(name + " is " + age)  # TypeError: can't concatenate str and int
print(name + " is " + str(age))

# print(10 + "20")  # TypeError: unsupported operand types
print(10 + int("20"))
```

### String Methods

```py
greeting = " Hello, Ninjas! "

print(len(greeting))

# Non-destructive methods (original string remains unchanged)
print(greeting.strip())
print(greeting.strip().lower())
print(greeting.strip().upper())
print(greeting.replace("Hello", "Yo").strip())
```

### Comments

- `#` for single-line comments
- `'''...'''` for multi-line comments

## lists & tuples

### lists

```py
names = ['mario', 'peach', 'luigi']

print(names[0])
# access list item

print('length of the list is:', len(names))
# get number of items in list
```

### changing list values

```py
names[1] = 'toad'
# change value of item at index
```

### list methods

```py
names.append('bowser')
# add item to end of list

names.remove('luigi')
# remove item from list

names.sort()
# sort list (alphabetically by default)
```

### tuples

```py
top_scores = (100, 95, 92, 92, 88, 85)

print(top_scores[0])
# access tuple item

print('length of the tuple is:', len(top_scores))
# get list of items in tuple
```

### tuples and immutable

```py
# top_scores[0] = 99 # TypeError: 'tuple' object does not support item assignment
```

### tuples methods

```py
print(top_scores.count(92))
# count number of items in tuple

print(top_scores.index(85))
# get index of item in tuple
```

### joining and slicing lists

### joining lists

```py
ages_one = [25, 30, 49, 60, 75]
ages_two = [19, 65, 21, 44, 38]

joined_ages = ages_one + ages_two
# join the two lists together
# does not mutate the original lists

ages_one.extend(ages_two)
# extend the first list with the second list
# mutates the first list

print('ages_one extended: ', ages_one)
```

### slicing lists

```py
scores = [100, 99, 25, 44, 85, 77, 64]

print('from start to index 2: ', scores[:3])
print('from index 3 to end:', scores[3:])
print('from index 1 to 4:', scores[1:4])
print('from start to 4 with step of 2:', scores[:4:2])
print('from start to end with step of 2:', scores[::2])
print('reversing the list:', scores[::-1])
```

## dictionaries
