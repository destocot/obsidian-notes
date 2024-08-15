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

print(len(greeting))  # 17

# Non-destructive methods
print(greeting.strip())
print(greeting.strip().lower())
print(greeting.strip().upper())
print(greeting.replace("Hello", "Yo").strip())
print(greeting)  # Original string remains unchanged
```

### Comments

- `#` for single-line comments
- `'''...'''` for multi-line comments

## lists & tuples
