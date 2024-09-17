---
title: "Third Party Packages & pip"
sidebar:
  order: 14
---

## pip & PyPi

### The Python Package Index

[https://pypi.org/](https://pypi.org/)

**pip** - pip installs packages

```bash
pip --version
# pip 22.0.2 from /usr/lib/python3/dist-packages/pip (python 3.10)
```

## Installing the Pendulum Package

The **pendulum** package makes it easier to work with datetimes.

```bash
pip install pendulum
```

> By default, this package will be installed globally on our system.

```bash
pip list
# list packages installed (globally)
```

```py
import pendulum


def main():
    pdt = pendulum.now()
    print(pdt)  # 2024-09-17 10:20:42.623051-04:00

    # formatting with pendulum
    print(pdt.format("MM/DD/YYYY"))  # 09/17/2024
    print(pdt.format("H:m A"))  # 10:20 AM
    print(pdt.to_day_datetime_string())  # Tue, Sep 17, 2024 10:20 AM

    # other pendulum methods
    pdt2 = pdt.add(days=1, months=2, years=3)
    print(pdt2.to_day_datetime_string())  # Thu, Nov 18, 2027 10:20 AM

    pdt3 = pdt.subtract(years=4, weeks=1)
    print(pdt3.to_day_datetime_string())  # Thu, Sep 10, 2020 10:20 AM

    pdt4 = pdt.set(year=1986, month=4)
    print(pdt4.to_day_datetime_string())  # Thu, Apr 17, 1986 10:20 AM

if __name__ == "__main__":
    main()
```

## Virtual Environments

Drawbacks of globally installing packages

- version conflicts
- bloats global namespace

### Create Virtual Enviroment

```bash
# Create Virtual Environment
python -m venv .env

# Activate Virtual Environment
source ./.venv/bin/activate

# Deactivate Virtual Environment
deactivate
```

## Installing Packages in a Virtual Environment

The **rich** package renders rich text, progress bars, syntax highlighting, markdown, and more to the terminal.

```bash
pip install rich
```

```py
from rich import print

def main():
    print("Hello, [underline red]Ninjas[/]!") # Hello, Ninjas!

if __name__ == "__main__":
    main()
```

## pip freeze & requirements.txt

A `requirements.txt` is used to help other users install dependencies when working with your project.

requirements.txt

```
markdown-it-py==3.0.0
mdurl==0.1.2
Pygments==2.18.0
rich==13.7.1
```

```bash
pip install -r requirements.txt
```
