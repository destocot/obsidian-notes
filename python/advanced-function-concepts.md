---
title: "Advanced Function Concepts"
sidebar:
  order: 11
---

## Higher Order Functions

more resuable and modular

```py
def ninja_action(action, x):
    return action("ninja", x)

def attack(character, x):
    return f"{character} attacks with a strength of {x}"

def defend(character, x):
    return f"{character} defends with a block power of {x}"

action_one = ninja_action(attack, 5)
action_two = ninja_action(defend, 7)

print(action_one)  # ninja attacks with a strength of 5
print(action_two)  # ninja defends with a block power of 7
```

### build-in higher order functions

```py
# map function
moves = ["punch", "kick", "block"]

def make_move_powerful(move):
    return f"{move.upper()}!!"

powerful_moves = map(make_move_powerful, moves)
print(list(powerful_moves)) # ['PUNCH!!', 'KICK!!', 'BLOCK!!']
```

```py
# filter function
scores = [20, 95, 45, 85, 90, 15, 55, 100, 10]

def is_high_score(score):
    return score >= 90

high_scores = filter(is_high_score, scores)
print(list(high_scores))  # [95, 90, 100]
```

## Lambda Functions

```py
def main():
    # example 1
    add_stuff = lambda x, y: 10 + x + y
    result = add_stuff(5, 7)
    print(result)

    # example 2 - with build in HO function
    scores = [20, 95, 45, 85, 90, 15, 55, 100, 10]

    high_scores = filter(lambda score: score >= 90, scores)
    print(high_scores)
    print(list(high_scores))

    return

if __name__ == "__main__":
    main()
```

## Decorators

add functionality to another function

```py
def luigi_move(func):
    def wrapper():
        print("Luigi is preparing a move...")
        func()
        print("Move complete!")

    return wrapper

@luigi_move
def stealth_attack():
    print("Performing a stealth attack!")

def main():
    stealth_attack()

if __name__ == "__main__":
    main()
```

real-world use of decorators

- `@require_auth` - check user auth before func conditionally runs

- `@validate_input` - check & validate func arguments before func runs
- `@preprocess` - modify func arguments to be in a specific format

## Closures

a closure is a function that remembers values from its enclosing scope

```py
def move_factory(character_name):
    uppercase_name = character_name.upper()

    def print_move(move_name):
        print(f"{uppercase_name} performs {move_name}!")

    return print_move


def main():
    ryu_move = move_factory("Ryu")
    ryu_move("Sweeping Slash")
    return


if __name__ == "__main__":
    main()
```
