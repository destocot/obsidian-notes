- language used to make **multi-platform** applications
- compiled into Machine Code (or JavaScript for web)
- is a **statically typed language**

[Dart Pad](https://dartpad.dev/?)
## Basics

- Dart will look for a `void main() { }` function as an entry point
- Cannot change value to a different data type after it has been initialized

- var keyword
```dart
var name = "mario"; // can only be reassigned to another string
```

- final keyword (runtime constant)
	- when we don't know what the value will be at compiled time
```Dart
final age = 25; // treated as a constant, value cannot be reassigned
```

- const keyword (compiled time constant)
	- when we know what the value is at compiled time
```Dart
const isOpen = true; // treated as a constant, value cannot be reassigned
```

- print function
```Dart
print(name);
print("my name is " + name); // concatenation
print("my name is $name"); // interpolation
print("my name is ${name}"); // interpolation (braces for objects)
```

- comments
```Dart
// single line

/* multi 
line */
```

- arithmetic
```Dart
const age = 25;
print(age + 10); // 35
print(age - 10); // 15
print(age * 10); // 250
print(age / 6); // 4.166666666666667
print(age % 6); // 1
```




## Type Annotations

## Functions

## Lists & Sets

## Control Flow

## Maps

## Classes

## Method Overriding

## Generics

## Async, Await & Futures

## Fetching Data

