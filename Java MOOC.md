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
```
// if you don't need to keep track of the indexes
ArrayList<String> teachers = new ArrayList<>();

for (String techer: teachers) {
	System.out.println(teacher);
}
```

### remove from a list
```
list.remove(<index>);

list.remove(<value>);
```

> to remove an integer directly use the `Integer.valueOf(<number>)` method

### check for existence of a value in a list
```
list.contains(<value>); // returns a boolean
```

---
# Arrays

- an array contains a limited amount of numbered spots (indices) for values

### creating an array
```
<type>[] array = new <type>[<size>]

<type>[] array = { arrayElement1, arrayElement2, ... }
```

### assigning and accessing elements
```
array[<index>] = value
```

### swapping elements in an array
```
int temporary = array[index1];
array[index1] = array[index2];
array[index2] = temporary;
```

### size of an array
```
array.length
```

---

# Strings

- Strings can't be compared with the equals operator `==` 
- To compare strings one should use the `equals` command
```
string1.equals(string2)
```

### Splitting a string
- the `split` method returns an array of the resulting sub-parts
```
String[] pieces = text.split(" ");
```

### ==example== If a string contains another string
- `contains` method
```
String text = "volcanologist";
text.contains("can");
```

### Getting a character at a specific index
```
text.charAt(<index>);
```

### Length of a string
```
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

```
Person person = new Person("Khurram", 27);

```

- the keyword **private** means that the variables are "hidden" inside the object.
	- this is known as **encapsulation**
```
public class Person {
	private String name;
	private int age;
}
```

### Constructor
- a method that creates the object
- the constructor's name is always the same as the class name
- objects are always created using a constructor
```
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
> the default constructor does not initialize any variables; object references will be `null` and primatives will be `0`

### Static
- the `static` modifier indicates that the method in question does not belong to an object
	- thus cannot be used to access any variables that belong to objects.

### `toString` method
- the method to return a "string representation" of an object is always called `toString` in Java.
```
public class Person {
	this.name;
	this.age;

	public String toString() {
		return this.name + ", age " + this.age + " years";
	}
}
```

```
Person pekka = new Person("Pekka");
System.out.println(pekka); // this is the same as System.out.println(pekka.toString());
```

> Java adds the call to the `toString` method automatically.

### Getters
- it is convention in Java to name a method that returns an instance variable in this manner
```
public <InstanceVariableType> get<InstanceVariableName>() { 
	return this.InstanceVariableName 
}
```

```
public class Person {
	private String name;

	public String getName() {
		return this.name;
	}
}
```

### Setters
 - a setter method's only purpose is to set a value to an instance variable
```
public void set<InstanceVariablename>(<InstanceVariableType> initialValue) {
	this.InstanceVariableName = initialValue;
}
```

### `final` keyword
- the final keyword means that the value of the variable cannot be modified after it has been set for the first time
```
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

---

# Files and reading data

### Reading a file
```
import java.util.Scanner;
import java.nio.file.Paths;
```

```
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
```
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

