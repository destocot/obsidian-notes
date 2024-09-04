---
title: "Functions and Modules 2"
sidebar:
  order: 7
---

## Arguments

```py
def multiply(a, b):
    return a * b

result = multiply(7, 5)

print(result)
```

### named arguments

```py
def print_score(name, score):
    print(f'{name} has a score of {score}')


print_score(score=75, name='yoshi')
```

### default arguments

```py
def divide(a, b = 2):
    return a / b

result = divide(20, 4)
print(result)

result = divide(50)
print(result)
```

## Unpacking Operator

### use \*args to accept any number of arguments

```py
def print_total(*args):
    print(args)  # args is a tuple

    total = 0
    for number in args:
        total += number
    print(f"the total is: {total}")

print_total(50, 75)
print_total(24, 35, 79, 100, 15)
```

### use \*kwargs to accept any number of keyword arguments

```py
def print_ninja(**kwargs):
    print(kwargs)  # kwargs is a dictionary

    for key, value in kwargs.items():
        print(f"{key} -- {value}")
    return

print_ninja(name="yoshi", age=25)
print_ninja(first_name="mario", belt_color="white")
```

## Return Values

```py
def square(x):
    return x * x

result = square(5)
print(result)
```

### returning multiple values

```py
def get_coords():
    x = 25.5
    y = 48.2
    return x, y  # returns a tuple

x, y = get_coords()
print(x, y)
```

### using return to break out of a function

```py
age = 15

def do_something():
    if age < 20:
        return

    print(age)

do_something() # nothing is printed
```
