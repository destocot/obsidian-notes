---
title: "Conditional Flow"
sidebar:
  order: 4
---

## Conditioanl Statements

````py
score = 7

### if statements

```py
if score > 5:
  print('the score is greater than 5')
````

### if/else statements

```py
if score > 10:
  print('the score is greater than 10')
else:
  print('the score is not greater than 10')
```

### if/elif/else statements

```py
if score > 10:
  print('the score is greater than 10')
elif score > 5:
  print('the score is greater than 5 but not greater than 10')
else:
  print('the score is 5 or less')
```

### and, or, not, is not

```py
age = 18

if age > 12 and age < 20:
  print('teenager')

if age < 13 or age > 19:
  print('not a teenager')

authenticated = False

if authenticated:
  print('you are authenticated')

if not authenticated:
  print('you are not authenticated')

if authenticated is True:
  print('you are authenticated')

if authenticated is not True:
  print('you are not authenticated')
```

## Conditional Assignments

```py
score = int(input("Enter a score between 0 & 100: "))
```

### conditional assignment / ternary

```py
is_top_score = True if score >= 90 else False
print('is_top_score:', is_top_score)
```

### nested ternary

```py
temp = int(input("Enter a temp in celsius between 0 & 40: "))

weather = 'hot' if temp > 25 else 'mild' if temp > 15 else 'cold'
print('weather:', weather)
```

## While Loops

```py
score = 0

while score < 10:
  print('the score is:', score)
  score += 1
```

```py
user_input = ''

while user_input != 'q':
  user_input = input("Enter a letter or enter 'q' to quit: ")
  print('You entered:', user_input)
```

## Break and Continue

### break

--> break out of loop

```py
count = 0

while count < 10:
  if count == 5:
    break

  print(count)
  count += 1
```

### continue

--> skip an iteration

```py
count = 0

while count < 10:
  count += 1

  if count % 2 != 0:
    continue

  print(count)
```
