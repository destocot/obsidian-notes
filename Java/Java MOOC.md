# Lists

### import the list to make it available for the program
```
import java.util.ArrayList;
```

### create a new list
```
ArrayList<Type> list = new ArrayList<>();
```

> once a list has been created, ArrayList assumes that all variables contained in it are reference type ( Integer, Double, Boolean, String)

### add to a list
```
list.add(<value>);
```

### retrieve from list
```
list.get(<index>)
```

## size of a list
```
list.size();
```

### ==example== iterating over a list with a for-each loop
```java
// if you don't need to keep track of the indexes
ArrayList<String> teachers = new ArrayList<>();

for (String techer: teachers) {
	System.out.println(teacher);
}
```

### remove from a list
```java
list.remove(<index>);
list.remove(<value>);
```

> to remove an integer directly use the `Integer.valueOf(<number>)` method

### check for existence of a value in a list
```java
list.contains(<value>); // returns a boolean
```

### clear a list
```java
list.clear();
```
---
# Arrays

- an array contains a limited amount of numbered spots (indices) for values

### creating an array
```java
<type>[] array = new <type>[<size>]

<type>[] array = { arrayElement1, arrayElement2, ... }
```

### assigning and accessing elements
```java
array[<index>] = value
```

### swapping elements in an array
```java
int temporary = array[index1];
array[index1] = array[index2];
array[index2] = temporary;
```

### size of an array
```java
array.length
```

---

# Strings

- Strings can't be compared with the equals operator `==` 
- To compare strings one should use the `equals` command
```java
string1.equals(string2)
```

### Splitting a string
- the `split` method returns an array of the resulting sub-parts
```java
String[] pieces = text.split(" ");
```

### ==example== If a string contains another string
- `contains` method
```java
String text = "volcanologist";
text.contains("can");
```

### Getting a character at a specific index
```java
text.charAt(<index>);
```

### Length of a string
```java
text.length();
```

---

# Object-oriented Programming

- object-oriented programming is concerned with isolating concepts of a problem domain into separate entities and then using those entities to solve problems.

### Classes and Objects
- a **class** defines the attributes of objects (the information related to them; instance variables and methods)
- the **instance variables** define the internal state of an object
- a **method** is a piece of source code written inside a class that's been named and has the ability to be called.
	- a method is always *often* used to modify the internal state of an object instantiated from a class

> an object is always instantiated by called a method that created an object
> i.e. a **constructor** by using the **new** keyword

```java
Person person = new Person("Khurram", 27);
```

- the keyword **private** means that the variables are "hidden" inside the object.
	- this is known as **encapsulation**
```java
public class Person {
	private String name;
	private int age;
}
```

### Constructor
- a method that creates the object
- the constructor's name is always the same as the class name
- objects are always created using a constructor
```java
public class Person {
	private String name;
	private int age;
	
	public Person(String initialName) {
		this.age = 0;
		this.name = initialName;
	}
}
```

> if a constructor is not defined, Java automatically creates a default constructor
> the default constructor does not initialize any variables; object references will be `null` and primitives will be `0`

### Static
- the `static` modifier indicates that the method in question does not belong to an object
	- thus cannot be used to access any variables that belong to objects.

### `toString` method
- the method to return a "string representation" of an object is always called `toString` in Java.
```java
public class Person {
	this.name;
	this.age;

	public String toString() {
		return this.name + ", age " + this.age + " years";
	}
}
```

```java
Person pekka = new Person("Pekka");
System.out.println(pekka); // this is the same as 
System.out.println(pekka.toString());
```

> Java adds the call to the `toString` method automatically.

### Getters
- it is convention in Java to name a method that returns an instance variable in this manner
```java
public <InstanceVariableType> get<InstanceVariableName>() { 
	return this.InstanceVariableName 
}
```

```java
public class Person {
	private String name;

	public String getName() {
		return this.name;
	}
}
```

### Setters
 - a setter method's only purpose is to set a value to an instance variable
```java
public void set<InstanceVariablename>(<InstanceVariableType> initialValue) {
	this.InstanceVariableName = initialValue;
}
```

### `final` keyword
- the final keyword means that the value of the variable cannot be modified after it has been set for the first time
```java
public class PaymentCard {
	private double balance;
	private final double affordable;

	public PaymentCard(double initialBalance) {
		this.balance = initialBalance;
		this.affordable = 2.6;
	}

	public void eatAffordably() {
		this.balance -= this.affordable;
	}
}
```

### ==example== program built from small and distinct objects

- `ClockHand` class
```java
public class ClockHand {
    private int value;
    private int limit;

    public ClockHand(int limit) {
        this.limit = limit;
        this.value = 0;
    }

    public void advance() {
        this.value = this.value + 1;

        if (this.value >= this.limit) {
            this.value = 0;
        }
    }

    public int value() {
        return this.value;
    }

    public String toString() {
        if (this.value < 10) {
            return "0" + this.value;
        }

        return "" + this.value;
    }
}
```

- Clock class with instance variables of the `ClockHand` class
```java
public class Clock {
    private ClockHand hours;
    private ClockHand minutes;
    private ClockHand seconds;

    public Clock() {
        this.hours = new ClockHand(24);
        this.minutes = new ClockHand(60);
        this.seconds = new ClockHand(60);
    }

    public void advance() {
        this.seconds.advance();

        if (this.seconds.value() == 0) {
            this.minutes.advance();

            if (this.minutes.value() == 0) {
                this.hours.advance();
            }
        }
    }

    public String toString() {
        return hours + ":" + minutes + ":" + seconds;
    }
}
```

- abstracted away during implementation
```java
Clock clock = new Clock();

while (true) {
    System.out.println(clock);
public void growOlder() {
    this.growOlder(1);
}

public void growOlder(int years) {
    this.age = this.age + years;
}    clock.advance();
}
```

### Constructor Overloading
- the technique of having two (or more) constructors in a class is known as *constructor overloading*. A class can have multiple constructors that differ in the number and/or type of their parameters
```java
public class Person {
	public Person(String name) {
	        this.name = name;
	        this.age = 0;
	    }
	
	public Person(String name, int age) {
	    this.name = name;
	    this.age = age;
	}
}
```

```java
public static void main(String[] args) {
    Person paul = new Person("Paul", 24);
    Person ada = new Person("Ada");
}
```

> DRY
```java
public Person(String name) {
    this(name, 0);
}

public Person(String name, int age) {
    this.name = name;
    this.age = age;
}
```

### Overloading Methods
- methods can be overloaded in the same way as constructors
```java
public void growOlder() {
    this.age = this.age + 1;
}

public void growOlder(int years) {
    this.age = this.age + years;
}
```

> DRY
```java
public void growOlder() {
    this.growOlder(1);
}

public void growOlder(int years) {
    this.age = this.age + years;
}
```

### `null` value of a reference variable
```java
Person joan = new Person("Joan Ball");
Person ball = joan;
joan = new Person("Joan B.");
ball = null;
```

- in Java the programmer need not worry about the program's memory use. The automatic garbage collector of the Java language cleans up the objects that have become garbage (in this example the object whose name is Joan Ball).
### `NullPointerException`
```java
Person joan = new Person("Joan Ball");
joan = null;
joan.growOlder();
```

```java
Exception in thread "main" java.lang.NullPointerException
at Main.main(Main.java:(row))
Java Result: 1
```

### Check if two objects are of the same type have the same contents or state
- if we want to be able to compare two objects of our own design with the `equals` method, that method must be defined in the class.
```java
public class SimpleDate {
    private int day;
    private int month;
    private int year;

    public SimpleDate(int day, int month, int year) {
        this.day = day;
        this.month = month;
        this.year = year;
    }

    public boolean equals(Object compared) {
        // if the variables are located in the same position
        // they are equal
        if (this == compared) {
            return true;
        }

        // if the type of the compared object is not SimpleDate
        // the objects are not equal
        if (!(compared instanceof SimpleDate)) {
            return false;
        }

        // convert the Object type compared object
        // into a SimpleDate type object called comparedSimpleDate
        SimpleDate comparedSimpleDate = (SimpleDate) compared;

        // if the values of the object variables are the same
        //the objects are equal
        if (this.day == comparedSimpleDate.day &&
            this.month == comparedSimpleDate.month &&
            this.year == comparedSimpleDate.year) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
    }
}
```

### Object equality and lists
- The `contains` method of a list uses the `equals` method that is defined in the object.
- If no equals method is created than each instance `list.contains(object)` will result in `false`.

### Returning an Object
```java
public class Factory {
    private String make;

    public Factory(String make) {
        this.make = make;
    }

    public Car produceCar() {
        return new Car(this.make);
    }
}
```



---

# Primitives and Reference Variables

#### Primitives
- Java has eight different primitive variables
	- boolean, byte, char, short, int, long, float, and double
- Declaring a primitive variable causes the computer to reserve some memory where the value assigned to the variable can be stored.
- The value of variables are also **copied** whenever they're used in method calls.

> The most significant difference between primitive and reference variables is that primitives (usually numbers) are immutable.


#### Reference

- The value of a reference variable -- i.e., the reference -- points to a location that contains information relating to the given variable. As a method parameter the value is passed as a reference.

```java
public class Example {
    public static void main(String[] args) {
        Person first = new Person("First");

        System.out.println(first);
        youthen(first);
        System.out.println(first);

        Person second = first;
        youthen(second);

        System.out.println(first);
    }

    public static void youthen(Person person) {
        person.setBirthYear(person.getBirthYear() + 1);
    }
}
```

```
First (1970)
First (1971)
First (1972)
```

---
# Date

### `LocalDate` class
```java
import java.time.LocalDate;
```

```java
public static void main(String[] args) {
	LocalDate now = new LocalDate.now();
	int year = now.getYear();
	int month = now.getMonthValue();
	int day = now.getDayOfMonth();
}
```


---

# Files and reading data

### Reading a file
```java
import java.util.Scanner;
import java.nio.file.Paths;
```

```java
try (Scanner scanner = new Scanner(Paths.get("file.txt"))) {
	while (scanner.hasNextLine()) {
		String row = scanner.nextLine();
		System.out.println(row);
	}
} catch (Exception e) {
	System.out.pritnln("Error: " + e.getMessage());
}
```

### Dealing with empty lines in a file
- sometimes an empty line finds its way into a file
- skipping an empty line can be done using the command `continue` and `isEmpty` method
```java
try (Scanner scanner = new Scanner(Paths.get("henkilot.csv"))) {
	while (scanner.hasNextLine()) {
		String line = scanner.nextLine();
		if (line.isEmpty()) {
			continue;
		}
		// do something with the data
	}
} catch (Exception e) { ... }
```

