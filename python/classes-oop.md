---
title: "Classes (OOP)"
sidebar:
  order: 12
---

## Defining Classes

```py
class User:
    # constructor
    def __init__(self, username, email):
        self.username = username
        self.email = email

def main():
    user_one = User("leo", "leo@example.com")
    user_two = User("raph", "raph@example.com")

    print("user 1:")
    print(user_one.username, user_one.email)

    print("user 2:")
    print(user_two.username, user_two.email)

    user_one.email = "leo4eva@example.com"
    print(user_one.email)

if __name__ == "__main__":
    main()
```

## Class Methods

```py
class User:
    def __init__(self, username, email):
        self.username = username
        self.email = email

    # instance method
    def post_status(self, status):
        print(f"{self.username} posted: {status}")

def main():
    user_one = User("leo", "leo@example.com")
    user_two = User("raph", "raph@example.com")

    user_one.post_status("Hello ninjas! This is Leo!")
    user_two.post_status("Raph here, ready to rumble!")

if __name__ == "__main__":
    main()
```

## Static Methods

```py
class User:
    def __init__(self, username, email):
        self.username = username
        self.email = email

    def post_status(self, status):
        print(f"{self.username} posted: {status}")

    # static method
    @staticmethod
    def validate_email(email):
        return "@" in email and len(email) > 3

def main():
    user_one = User("leo", "leo@example.com")
    user_two = User("raph", "raph@example.com")

    print(User.validate_email(user_one.email)) # True

if __name__ == "__main__":
    main()
```

## Inheritance

```py
class User:
    def __init__(self, username, email):
        self.username = username
        self.email = email

    # instance method
    def post_status(self, status):
        print(f"{self.username} posted: {status}")

    # static method
    @staticmethod
    def validate_email(email):
        return "@" in email and len(email) > 3


# inheritance
class SuperUser(User):
    def __init__(self, username, email, avatar):
        super().__init__(username, email)
        self.avatar = avatar

    def post_announcement(self, message):
        print(f"Site announcement from {self.username}: {message}")


def main():
    user_one = User("leo", "leo@example.com")
    user_two = User("raph", "raph@example.com")
    super_user = SuperUser("splinter", "splinter@example.com", "splinter.jpg")

    super_user.post_announcement("Hey, splinter here with some news.")


if __name__ == "__main__":
    main()
```
