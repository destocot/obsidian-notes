---
title: "Functions and Modules 3"
sidebar:
  order: 8
---

## The Main Function

```py
def main():
    # The main function acts as the entry point of the script.
    # It is a convention in Python to have a main() function
    # that handles the execution logic of the script.
    print("hello from inside the main function")

if __name__ == "__main__":
    # This condition checks if the script is being run directly.
    # If true, it calls the main() function to execute the script.
    # This is crucial for scripts that may be imported as modules
    # elsewhere, ensuring that the main() function only runs when
    # the script is executed directly, not when imported.
    main()
```

## Using Modules

```py
# app.py
from helpers import add, multiply

def main():
    print("hello from inside the main function, in the app file")
    result = add(2, 7)
    print(result)

    result = multiply(3, 4)
    print(result)

if __name__ == "__main__":
    main()
```

```py
# helpers.py
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

print(__name__)

def main():
    print("hello from inside the main function, in the helpers file")

if __name__ == "__main__":
    main()
```

## CHALLENGE - Contacts

```py
contacts = []

# CHALLENGE - create an add_contact function to add new contacts to the list
# each contact should be a dictionary with a name and email property

def add_contact(name, email):
    newContact = {"name": name, "email": email}
    contacts.append(newContact)

# CHALLENGE - create a list_contacts function to print out all contacts
# each contact should be printed on a new line with a name & email
# if there are no contacts yet, print out a 'no contacts yet' message

def list_contacts():
    if len(contacts) == 0:
        print("There are no contacts yet.")
        return

    for contact in contacts:
        print(f'{contact["name"]} -- {contact["email"]}')

def main():
    while True:
        print("Press 1 to add a contact, press 2 to list contacts.")
        choice = input("Your choice: ")

        if choice == "1":
            print("You chose option 1.")
            name = input("New contact name: ")
            email = input("New contact email: ")
            add_contact(name, email)
            continue

        elif choice == "2":
            print("You chose option 2.")
            list_contacts()
            continue

        else:
            print("Invalid choice. Exiting program.")
            break

if __name__ == "__main__":
    main()
```

## Python Standard Library

[The Python Standard Library](https://docs.python.org/3/library/index.html)

```py
from random import randint, shuffle

def main():
    number = randint(1, 1000)
    print(f"random number is {number}")

    numbers = [1, 2, 3, 4, 5, 6, 7]
    shuffle(numbers)
    print(f"shuffled numbers: {numbers}")

if __name__ == "__main__":
    main()
```
