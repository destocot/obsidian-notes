---
title: "Conditional Flow 2"
sidebar:
  order: 5
---

## For Loops

--> iterate a collection or range

```py
names = [ "yoshi", "mario", "luigi", "peach" ]

for name in names:
  print(name)
```

```py
names = [ "yoshi", "mario", "luigi", "peach" ]

for name in names:
  if name == 'luigi':
    break

  print(name)
```

```py
names = [ "yoshi", "mario", "luigi", "peach" ]

for name in names:
  if name == 'mario':
    continue

  print(name)
```

### for loops with strings

```py
for letter in 'ninja':
  print(letter.upper())
```

## Ranges with Loops

### for loops with ranges

```py
# print numbers from 0 to 4
for i in range(5):
    print(i)

# print numbers from 10 to 19
for i in range(10, 20):
    print(i)

# print numbers from 10 to 19 with step 2
for i in range(10, 20, 2):
    print(i)
```

### negative steps

```py
# print numbers from 10 to 1
for i in range(10, 0, -1):
    print(i)

# print numbers from 20 to 11 with step -2
for i in range(20, 10, -2):
    print(i)
```

## Match Statements

requires at least Python 3.10

```py
belt_color = input('What is your ninja belt color: ')

match belt_color:
  case 'white':
    print('Ninja Fledgling')
  case 'red':
    print('Intermediate Ninja')
  case 'blue':
    print('Advanced Ninja')
  case 'purple':
    print('Pro Ninja')
  case 'black':
    print('Master Ninja')
  case _:
    print('Belt color is unknown')
```

## Error Handling

### example without error handling

```py
number = int(input("Enter a number: "))
# if user enters a string, their will be a ValueError
```

### example 1

```py
try:
  number = int(input("Enter a number: "))
  print("You entered:", number)
except ValueError as e:
  print("That is not a number")
  print('error:', e)
except:
  print("An error occurred")
finally:
  print("Completed")
```

### example 2

```py
a = 10
b = 0

try:
  print(a / b)
except ZeroDivisionError as e:
  print("cannot divide a number by 10")
  print('error:', e)
except:
  print("An error occurred")

# we will reach this line because of our try-except block
print('end of the file')
```

## CHALLENGE - Shopping List

```py
shopping_list = []

# SECTION ONE - creating the shopping list
while True:
  item = input("Enter an item (or 'q' to quit): ")

  if item == 'q':
    break

  try:
    price = int(input("Enter the price(£) of the item: "))
  except ValueError:
    print('That is not a valid price.')
    continue

  shopping_list.append((item, price))

# SECTION TWO - formatting the shopping list

total = 0

for item, price in shopping_list:
  print(f"{item} - £{price}")
  total += price

print(f"total price: £{total}")
```
