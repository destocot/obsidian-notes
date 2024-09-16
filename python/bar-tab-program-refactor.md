---
title: "Bar Tab Program Refactor"
sidebar:
  order: 13
---

## MINI PROJECT - Bar Tab Program Refactor

```py
import csv
from pathlib import Path

class BarTab:
    def __init__(self, table_number):
        self.table_number = table_number
        self.drinks = []
        self.subtotal = 0
        self.tip = 0
        self.total = 0

    def serv_user(self):
        while True:
            drink_name = input("Drink name (or type 'f' to finish): ")

            if drink_name.lower() == "f":
                break

            try:
                cost = float(input(f"{drink_name} price: "))
                self.drinks.append((drink_name, cost))
            except ValueError:
                print("The price must be a number.")
                continue

    def calc_totals(self):
        for _, cost in self.drinks:
            self.subtotal += cost

        self.tip = self.subtotal * 0.2
        self.total = self.subtotal + self.tip

    def create_csv(self):
        path = Path(__file__).parent / f"table_{self.table_number}.csv"

        with path.open("w", newline="") as file:
            csv_writer = csv.writer(file)
            csv_writer.writerow(["Drink name", "Cost"])
            csv_writer.writerows(self.drinks)
            csv_writer.writerow(["Subtotal", self.subtotal])
            csv_writer.writerow(["Tip", self.tip])
            csv_writer.writerow(["Total", self.total])

            print(f"The bar tab has been saved to {path}")

def main():
    try:
        table_no = int(input("Table Number: "))
    except ValueError:
        print("Not a valid table number. Exiting program.")
        return

    bar_tab = BarTab(table_no)
    print(f"Starting a tab for table {bar_tab.table_number}")

    bar_tab.serv_user()
    print(bar_tab.drinks)

    if not bar_tab.drinks:
        print("No drinks added. Exiting program.")
        return

    bar_tab.calc_totals()
    bar_tab.create_csv()


if __name__ == "__main__":
    main()
```

```
Table Number: 7
Starting a tab for table 7
Drink name (or type 'f' to finish): water
water price: 3
Drink name (or type 'f' to finish): cola
cola price: 4
Drink name (or type 'f' to finish): mojito
mojito price: 9
Drink name (or type 'f' to finish): f
[('water', 3.0), ('cola', 4.0), ('mojito', 9.0)]
The bar tab has been saved to /home/destocot/pythonmc/table_7.csv
```
