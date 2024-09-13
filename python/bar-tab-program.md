---
title: "Bar Tab Program"
sidebar:
  order: 10
---

## MINI PROJECT - Bar Tab Program

```py
from pathlib import Path
from csv import writer

def create_csv(path, drinks, subtotal, tip, total):
    with path.open("w", newline="") as file:
        csv_writer = writer(file)
        csv_writer.writerow(["Drink name", "Cost"])
        csv_writer.writerows(drinks)
        csv_writer.writerow(["Subtotal", subtotal])
        csv_writer.writerow(["Tip", tip])
        csv_writer.writerow(["Total", total])

        print(f"The bar tab has been saved to {path}")

def calc_totals(drinks):
    subtotal = 0

    for name, cost in drinks:
        subtotal += cost

    tip = subtotal * 0.2
    total = subtotal + tip

    return subtotal, tip, total

def serv_user():
    drinks = []

    while True:
        drink_name = input("Drink name (or type 'f' to finish): ")

        if drink_name.lower() == "f":
            break

        try:
            cost = float(input(f"{drink_name} price: "))
            drinks.append((drink_name, cost))
        except ValueError:
            print("The price must be a number.")
            continue

    return drinks

def main():
    try:
        table_no = int(input("Table number: "))
        print(f"Starting a tab for table {table_no}")
    except:
        print("Not a valid table number. Exiting program.")
        return

    path = Path(__file__).parent / f"table_{table_no}.csv"
    drinks = serv_user()

    if not drinks:
        print("No drinks added. Exiting program.")
        return

    subtotal, tip, total = calc_totals(drinks)
    create_csv(path, drinks, subtotal, tip, total)

if __name__ == "__main__":
    main()
```

```
Table number: 21
Starting a tab for table 21
Drink name (or type 'f' to finish): white wine
white wine price: 7
Drink name (or type 'f' to finish): red wine
red wine price: 7
Drink name (or type 'f' to finish): water
water price: 3
Drink name (or type 'f' to finish): coke
coke price: 5
Drink name (or type 'f' to finish): f
The bar tab has been saved to /home/destocot/pythonmc/table_21.csv
```
