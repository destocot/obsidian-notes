---
title: "Working with Files"
sidebar:
  order: 9
---

## opening & reading files

```py
def read_file(filename):
    # open file
    file = open(filename, "r")

    # read file
    # content = file.read()
    # print(content)

    lines = file.readlines()

    for line in lines:
        print(line)

    # close file
    file.close()

def main():
    read_file('characters.txt')

if __name__ == "__main__":
    main()
```

- `open` takes in the filename relative to where the script is being ran
- `open` will throw an error if the file does not exist
- `'r'` will only put file in read mode

## writing to files

```py
characters = ["Mario", "Luigi", "Peach", "Yoshi", "Bowser", "Toad"]

def write_character_to_file(filename):
    # open file
    # relative to where the script is running
    file = open(filename, "w+")

    # write file
    for c in characters:
        file.write(c + "\n")

    file.seek(0)  # move cursor to beginning of file
    content = file.read()
    print(content)

    # close file
    file.close()

def main():
    write_character_to_file("characters.txt")

if __name__ == "__main__":
    main()
```

- `'w'` will only put file in write mode, `'w+'` will allow it to be in read an dwrite mode

## appending to files

```py
more_characters = ["Diddy Kong", "Donkey Kong", "Wario"]

def write_characters_to_file(filename):
    # open file
    file = open(filename, "a")

    # append to file
    for c in more_characters:
        file.write(c + "\n")

    # close file
    file.close()

def main():
    write_characters_to_file("characters.txt")

if __name__ == "__main__":
    main()
```

- `'a'` will put file in append mode, allowing us to write to a file without truncating it first and instead appending to the existing content

## working with paths

### pathlib module

```py
from pathlib import Path

file = open("characters.txt", "r")

def create_path():
    # get the directory of the script
    script_dir = Path(__file__).parent

    # create a path object
    path = script_dir / "characters"

    # create the directory if it does not exist
    path.mkdir(parents=True, exist_ok=True)

def main():
    create_path()

if __name__ == "__main__":
    main()
```

## Pathlib to Read & Write Files

### pathlib module

```py
from pathlib import Path

def create_path():
    # get the directory of the script
    script_dir = Path(__file__).parent

    # create a path object
    path = script_dir / "characters"

    # create the directory if it does not exist
    path.mkdir(parents=True, exist_ok=True)

    path = path / "zelda.txt"

    file = path.open("r")
    # content = file.read()
    # print(content)

    file.close()

    # convienient method to write text to a file
    # does not require to open and close the file
    path.write_text("Epona")

    # convienient method to read text from a file
    content = path.read_text()
    print(content)

def main():
    create_path()

if __name__ == "__main__":
    main()
```

## Handling File Errors

```py
from pathlib import Path

def open_file():
    path = Path(__file__).parent
    path = path / "does" / "not" / "exist.txt"

    try:
        file = path.open("r")
        content = file.read()
        print(content)
        file.close()
    except FileNotFoundError:
        print(f"{path} does not exist")
    except Exception as err:
        print(f"Unexpected error: {err}")

    print("end of function")

def main():
    open_file()

if __name__ == "__main__":
    main()
```

## Context Managers

```py
from pathlib import Path

def open_file():
    path = Path(__file__).parent / "characters.txt"
    data = ["Mario", "Luigi", "Peach", "Yoshi", "Bowser"]

    # context managers --> auto closes
    with path.open("w") as file:
        for character in data:
            file.write(character + "\n")

def main():
    open_file()

if __name__ == "__main__":
    main()
```

## Working with JSON Files

```py
from pathlib import Path
import json

path = Path(__file__).parent / "characters.json"

characters = {
    "characters": [
        {"name": "Mario", "age": 25},
        {"name": "Luigi", "age": 27},
        {"name": "Peach", "age": 26},
        {"name": "Bowser", "age": 35},
    ]
}


def write_json():
    with path.open("w") as file:
        json.dump(characters, file, indent=4)


def read_json():
    with path.open("r") as file:
        data = json.load(file)
        return data


def main():
    write_json()
    data = read_json()
    print(data)


if __name__ == "__main__":
    main()
```
