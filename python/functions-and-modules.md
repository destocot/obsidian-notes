---
title: "Functions and Modules"
sidebar:
  order: 6
---

## Defining Functions

```py
def greet():
  print('Hello, ninjas!')

greet()
```

### with a return value

```py
def say_hello():
  name = "mario"
  return f'Hello {name}'

result = say_hello()
print(result)
```

## Variable Scope

### global scope

```py
x = 10

def print_x():
  print(f"x inside the print_x func is {x}")

print_x() # x inside the print_x func is 10
print(f'global value of x is {x}') # global value of x is 10
```

### local scope

```py
x = 10

def print_x():
  x = 5 # new local variable x
  print(f"x inside the print_x func is {x}") # x inside the print_x func is 5

def print_y():
  y = 20
  print(f'y inside the print_y func is {y}') # y inside the print_y func is 20

print_y()
print_x()
print(f'global value of x is {x}') # global value of x is 10
```

### change global variable

```py
x = 10

def print_x():
  global x # global keyword is used to access the global variable
  x = 5
  print(f"x inside the print_x func is {x}") # x inside the print_x func is 5

def print_y():
  y = 20
  print(f'y inside the print_y func is {y}') # y inside the print_y func is 20

print_y()
print_x()
print(f'global value of x is {x}') # global value of x is 5
```

### enclosing scope (scope within nested functions)

```py
def outer():
  age = 25

  def inner():
    print(f'age inside inner() is: {age}')

  inner()
  print(f'age inside outer() is: {age}')

outer()
```

```
age inside inner() is 25
age inside outer() is 25
```

```py
def outer():
  age = 25

  def inner():
    age = 30 # this is a new variable, not the same as the one in outer()
    print(f'age inside inner() is {age}')

  inner()
  print(f'age inside outer() is {age}')

outer()
```

```
age inside inner() is 30
age inside outer() is 25
```

```py
def outer():
  age = 25

  def inner():
    nonlocal age # nonlocal keyword is used to declare that the variable is not local to the inner function

    age = 30
    print(f'age inside inner() is {age}')

  inner()
  print(f'age inside outer() is {age}')

outer()
```

```
age inside inner() is 30
age inside outer() is 25
```
