**TABLE OF CONTENTS**
- Hash Map
- Inheritance
- Polymorphism
- Abstract Classes
- Method to Test for Equality
- Interfaces
- Handling collections as streams
- Lambda Expressions
- Files and Streams
- The Comparable Interface
- Sorting Method
- Measure Performance Time

---

## Hash Map
- a hash map is used whenever data is stored as key-value pairs, where values can be added, retrieved, and deleted using keys.
- if the hash map does not contained they key used for the search, its `get` method returns a `null` reference
- two parameters are required when creating a hash map - the type of the key and the type of integer
```java
import.java.util.Hashmap;

HashMap<String, Integer> hashmap = new HashMap<>();
```

- add to hash map
```java
hashmap.put(/* key */, /* value */);
```

- get from hash map
```java
hashmap.get(/* key */);
```

- check if key exists in hash map
```java
hashmap.containsKey(/* key */); // returns true or false
```

- get set of keys in hash map
```java
Set<T> items = hashmap.keySet();

for (<T> item: items)  /* ... */}
```
	 
- get set of keys in hash map
```java
Set<T> keys = hashmap.keySet();

for (<T> key: keys)  /* ... */}
```

- get set of values in hash map
```java
Set<T> values = hashmap.keySet();

for (<T> value: values)  /* ... */}
```

> hash maps expect that only reference-variables are added to it. Java converts primitive variables to their corresponding reference-types when using any Java's built in data structures (this is termed auto-boxing in Java).

```java
HashMap<Integer, String> hashmap = new HashMap<>(); // works
HashMap<int, String> map2 = new HashMap<>(); // doesn't work
```

- for performing automatic conversion, we should ensure that the value to be converted is not `null`.
- the `getOrDefault` method of the HashMap searched for the key passed to it as a parameter from the HashMap. If the key is not found, it returns the value of the second parameter passed to it.
```java
public int timesSighted(String sighted) {
	return this.allSightings.getOrDefault(sighted, 0);
}
```
---
## Inheritance

- here Part represents the **superclass**
```java
public class Part {

    private String identifier;
    private String manufacturer;
    private String description;

    public Part(String identifier, String manufacturer, String description) {
        this.identifier = identifier;
        this.manufacturer = manufacturer;
        this.description = description;
    }

    public String getIdentifier() {
        return identifier;
    }

    public String getDescription() {
        return description;
    }

    public String getManufacturer() {
        return manufacturer;
    }
}
```

- here Engine represents the **subclass**
```java
public class Engine extends Part {

    private String engineType;

    public Engine(String engineType, String identifier, String manufacturer, String description) {
        super(identifier, manufacturer, description);
        this.engineType = engineType;
    }

    public String getEngineType() {
        return engineType;
    }
}
```

- in use
```java
Engine engine = new Engine("combustion", "hz", "volkswagen", "VW GOLF 1L 86-91");
System.out.println(engine.getEngineType()); // combustion
System.out.println(engine.getManufacturer()); // volkswagen
```

## Access Modifiers (`private`, `protected`, `public`)
- if a method or variable has the access modifier `private`, it is visible only to the internal methods of that class.
	- Subclasses will not see it, and a subclass has no direct means to access it
- A subclass sees everything that is defined with `public` modifier in the superclass.
- If we want to define **some** variables or methods that are visible to the subclasses but invisible to everything else, we can use the access modifier `protected` to achieve this.

## `super`
- you can use the keyword `super` to call the constructor of the superclass
- the call receives as parameters the types of values that the superclass constructor requires
```java
public class Superclass {

    private String objectVariable;

	// if this constructor is used, passes the value to the second constructor
    public Superclass() {
        this("Example");
    }

    public Superclass(String value) {
        this.objectVariable = value;
    }

    public String toString() {
        return this.objectVariable;
    }
}
```

```java
public class Subclass extends Superclass {

    public Subclass() {
        super("Subclass");
    }
}
```

```java
Superclass sup = new Superclass();
Subclass sub = new Subclass();

System.out.println(sup); // Example
System.out.println(sub); // Subclass
```

## superclass method
- you can call the methods defined in the superclass by prefixing the call with `super`
```java
@Override
public String toString() {
	return super.toString() + "\n And let's add my own message to it!";
}
```

## Polymorphism
- regardless of the type of the variable, the method that is executed is always chosen based on the actual type of the object
- Objects are polymorphic, which means that they can be used via many different variable types

> Polymorphism, meaning "many forms", is a core concept in object-oriented programming. It lets you treat objects of different types in a uniform way, as long as they share a common parent class. Think of it like this: if you have a "`makeSound()`" method for a general "Animal" class, polymorphism allows a "Dog" to "bark" and a "Cat" to "meow" when that same method is called. This makes your code incredibly flexible—you can write instructions that work with many different animal types without needing to know the specifics of each one beforehand.

## Abstract Classes
```java
// abstract class
public abstract class *NameOfClass*

// abstract method
public abstract returnType nameOfMethod
```


**example**
```java
public abstract class Operation {

    private String name;

    public Operation(String name) {
        this.name = name;
    }

    public String getName() {
        return this.name;
    }

    public abstract void execute(Scanner scanner);
}
```

```java
public class PlusOperation extends Operation {

    public PlusOperation() {
        super("PlusOperation");
    }

    @Override
    public void execute(Scanner scanner) {
        System.out.print("First number: ");
        int first = Integer.valueOf(scanner.nextLine());
        System.out.print("Second number: ");
        int second = Integer.valueOf(scanner.nextLine());

        System.out.println("The sum of the numbers is " + (first + second));
    }
}
```

```java
public class UserInterface {

    private Scanner scanner;
    private ArrayList<Operation> operations;

    public UserInterface(Scanner scanner) {
        this.scanner = scanner;
        this.operations = new ArrayList<>();
    }

    public void addOperation(Operation operation) {
        this.operations.add(operation);
    }

    public void start() {
        while (true) {
            printOperations();
            System.out.println("Choice: ");

            String choice = this.scanner.nextLine();
            if (choice.equals("0")) {
                break;
            }

            executeOperation(choice);
            System.out.println();
        }
    }

    private void printOperations() {
        System.out.println("\t0: Stop");
        int i = 0;
        while (i < this.operations.size()) {
            String operationName = this.operations.get(i).getName();
            System.out.println("\t" + (i + 1) + ": " + operationName);
            i = i + 1;
        }
    }

    private void executeOperation(String choice) {
        int operation = Integer.valueOf(choice);

        Operation chosen = this.operations.get(operation - 1);
        chosen.execute(scanner);
    }
}
```

**in use**
```java
UserInterface userInterface = new UserInterface(new Scanner(System.in));
userInterface.addOperation(new PlusOperation());

userInterface.start();
```

- the greatest difference between interfaces and abstract classes is that abstract classes can contain object variables and constructors in addition to methods.

### Summary
- **Interfaces**: define a contract of what a class should do.
- **Inheritance**: Creates an "is-a" relationship. A Car is a Vehicle.
- **Abstract classes**: Provides a partial template and common functionality that subclasses must complete or inherit.
## Method to Test For Equality - "equals"
- the **equals method** checks by default whether the object given as a parameter has the same reference as the object it is being compared to.
```java
Book book1 = new Book("Book Title");
Book book2 = book1;
Book book3 = new Book("Book Title");

System.out.println(book1.equals(book2)); // true
System.out.println(book1.equals(book3)); // false
```

- creating a custom "equals" method
```java
public class Book {
    private String name;
    private String content;
    private int published;

	/* OTHER METHODS */

    @Override
    public boolean equals(Object comparedObject) {
        // if the variables are located in the same place, they're the same
        if (this == comparedObject) {
            return true;
        }

        // if comparedObject is not of type Book, the objects aren't the same
        if (!(comparedObject instanceof Book)) {
            return false;
        }

        // let's convert the object to a Book-object
        Book comparedBook = (Book) comparedObject;

        // if the instance variables of the objects are the same, so are the objects
        if (this.name.equals(comparedBook.name) &&
            this.published == comparedBook.published &&
            this.content.equals(comparedBook.content)) {
            return true;
        }

        // otherwise, the objects aren't the same
        return false;
    }
}
```

## Approximate Comparison With HashMap
- the `hashCode` method creates from the object a 'hash code', i.e., a number, that tells a bit about the object's content.
- if two objects have the same hash value they **MAY** be equal.
- String and Integer-type objects have conveniently ready-made `hashCode` methods implemented.
- The default implementation of the `hashCode` in the `Object` class is based on the object's reference
	- we can define our own `hashCode` method to deal with comparing the contents of objects
	- we should also be aware of any `NullPointerException` errors
```java
public int hashCode() {
	if (this.name == null) {
		return this.published;
	}

	return this.published + this.name.hashCode();
}
```


**Summary**
- for a class to be used as a HashMap's key, we need to define for it
	- the `equals` method, so that all equal or approximately equal objects cause the comparison to return true and all false for all the rest
	- the `hashCode` method, so that as few objects as possible end up with the same hash value

**HashMap Helper Methods**
```java
// If value does not exist for key, set it to a defaultValue
hashmap.putIfAbsent(<key>, <defaultValue>);

// If value does not exist for a key, return a default value
hashmap.getOrDefault(<key>, <defaultValue>);
```

---

## Interfaces
- interfaces define behavior through method names and their return values
- visibility attribute on interfaces is not marked explicitly as they're always `public`

- the classes that implement the interface decide *how* the methods defined in the interface are implemented
```java
public interface Readable {
	String read();
}
```

- a class implements the interface by adding the keyword *implements* after the class name followed by the name of the interface being implemented
```java
public class TextMessage implements Readable {
	private String sender;
	private String content;

	public TextMessage(String sender, String content) {
		this.sender = sender;
		this.content = content;
	}

	public String getSender() {
		return this.sender;
	}

	public String read() {
		return this.content;
	}
}
```

## Interfaces as Method Parameters
- the true benefits of interfaces are reaped when they are used as the type of parameter provided as method.
```java
public class Printer {
	public void print(Readable readable) {
		System.out.println(readable.read());
	}
}
```

> `Polymorphism` works with interfaces to create a powerful way to write code that is flexible and easy to change. Having an animal interface with a `makeSound()` method will allow us to create classes (Dog, Cat, Bird, etc.) that implement the `makeSound() `method. Since we have a contract without interface we can call the `makeSound()` method without knowing the exact animal we are currently working with.


## Built-in Interfaces
- commonly used interfaces: List, Map, Set, and Collection
- interface *abstracts* their inner functionality

#### The Set Interface
- in Java, sets always contain either 0 or 1 amounts of any given object.
```java
Set<string> set = new HashSet<>();
set.add("one");
set.add("one");
set.add("two");

for (String element: set) {
	System.out.println(element);
}
```

```
one
two
```
- the `HashSet` in no way assumes the order of a set of elements. If objects created from custom classes are added to the `HashSet` object, they must have both the `equals` and `hashCode` methods defined.
- the `keySet` method of a `HashMap` returns a set of elements that implements the `Set` interface.

#### The Collection Interface
- among other things, lists and sets are categorized as collection in Java
- the `values` method of a `HashMap` returns a set of elements that implements the `Collection` interface
---
# Handling collections as streams
- stream is a way of going through a collection (e.g. lists) of data such that the programmer determines the operations to be performed on each value.

==example== - print the number of positive integers divisible by three, and the average of all values
```java
Scanner scanner = new Scanner(System.in);
List<String> inputs = new ArrayList<>();

while (true) {
	String row = scanner.nextLine();
	if (row.equals("end")) {
		break;
	}

	inputs.add(row);
}

// counting the number of values divisible by three
long numbersDivisibleByThree = inputs.stream()
	.mapToInT(s -> Integer.valueOf(s))
	.filter(number -> number % 3 == 0)
	.count();

// working out the aberage
double average = inputs.stream()
	.mapToInt(s -> Integer.valueOf(s))
	.average()
	.getAsDouble()

// printing out the statistics
System.out.println("Divisible by three " + numbersDivisibleByThree);
System.out.println("Average number: " + average);
```

- A stream can be formed from any object the implements the `Collection` interface (e.g., ArrayList, HashSet, HashMap, etc.)

**List of  stream methods encountered**
- `stream()` - creates a stream on a collection that implements the `Collection` interface
**Terminal Operations**
- `average()` - Calculates the average of elements in the stream and returns an `OptionalDouble`.
- `count()` -  Counts the elements in the stream.
- `collect()` - Accumulates elements into a new collection (List, Set, Map, etc.).
- `forEach()` - Performs an action on each element.
- `reduce()` -  Combines elements into a single value using a provided operation.
- `sum()` 

**Intermediate Operations**
- `map()` -  Transforms elements from one type to another.
	- `mapToInt()`: (A more specific form of `map`) Transforms elements to `int` values.
- `filter()` -  Keeps elements matching a condition, discarding the rest.
- `distinct()` - Removes duplicate elements.
	- uses the `equals`-method that is in all objects
- `sorted()` -  Sorts elements (either naturally or according to a comparator).
## Lambda Expressions
- is shorthand provided by Java for anonymous methods that do not have an owner
  (they are not part of a class or an interface)
- the function contains both the parameter definition and the function body

- shorthand (original)
```java
*stream*.filter(value -> value > 5).*furtherAction*
```

- writing out the full function
```java
*stream*.filter((Integer value) -> {
    if (value > 5) {
        return true;
    }

    return false;
}).*furtherAction*
```

- extracting the full function
```java
public class Screeners {
    public static boolean greaterThanFive(int value) {
        return value > 5;
    }
}

*stream*.filter(value -> Screeners.greaterThanFive(value)).*furtherAction*
```

- passing function directly as a parameter
```java
public class Screeners {
    public static boolean greaterThanFive(int value) {
        return value > 5;
    }
}

*stream*.filter(Screeners::greaterThanFive).*furtherAction*
```

> functions that handle stream elements cannot change values of variables **outside** of the function

## Stream Methods
- intermediate operations: for processing elements
- terminal operations: end the processing of elements

==example== Gather numbers that are divisible by two, three, or five
```java
public static ArrayList<Integer> divisible(ArrayList<Integer> numbers) {
	return numbers.stream()
		.filter(n -> n % 2 == 0 || n % 3 == 0 || n % 5 == 0)
		.collect(Collectors.toCollection(ArrayList::new));
}
```

`reduce` method - useful when you want to combine stream elements to some other form
- `reduce(*initialState*, (*previous*, *object*) -> *actions on the object*)`

==example== calculate the sum of an integer list
```java
ArrayList<Integer> values = new ArrayList<>();
values.add(7);
values.add(3);
values.add(2);
values.add(1);

int sum = values.stream()
	.reduce(0, (previousSum, value) -> previousSum + value);

System.out.println(sum);
```

==example== unique last names in alphabetical order
```java
persons.stream()
	.map(p -> p.getLastName())
	.distinct()
	.sorted()
	.forEach(System.out::println);
```

==example== calculating the average of the author's birth years
```java
double average = books.stream()
    .map(book -> book.getAuthor())
    .mapToInt(author -> author.getBirthYear())
    .average()
    .getAsDouble();
```

==example== filtering authors who have books with the word "Potter" in the titles
```java
books.stream()
    .filter(book -> book.getName().contains("Potter"))
    .map(book -> book.getAuthor())
    .forEach(author -> System.out.println(author));
```

## Files and Streams
- Streams are also very handy in handling files.

1. The file is read in stream form using Java's ready-made `Files` class.
2. The `lines` method in the files class allows you to create an input stream from a file, allowing you to process the rows one by one.
3. The `lines` method gets a path as its parameter, which is created using the `get` method in the `Paths` class.

==example== Reading books from a file
```java
List<String> Books = new ArrayList<>();

try {
	Files.lines(Paths.get("file.txt"))
		.map(row -> row.split(","))
		.filter(parts -> parts.length >= 4)
		.map(parts -> new Book(
			parts[0], 
			Integer.valueOf(parts[1]), 
			Integer.valueOf(parts[2]), 
			parts[3])
		)
		.collect(Collectors.toList());
} catch (Exception e) {
	System.out.println("Error: " + e.getMessage());
}
```

# The Comparable Interface
- the comparable interface defines the `compareTo` method used to compare objects
- the sorting result from the `compareTo` method is called *natural ordering*

==example== Member class implements the `Comparable` interface (sort by height)
```java
public class Member implements Comparable<Member> {
	private String name;
	private int height;

	public Member(String name, int height) {
		this.name = name;
		this.height = height;
	}

	public String getName() {
		return this.name;
	}

	public int getHeight() {
		return this.height;
	}

	@Override
	public String toString() {
		return this.getName() + "(" + this.getHeight() + ")";
	}

	@Override
	public int compareTo(Member member) {
		if (this.height == member.getHeight()) {
			return 0;
		} else if (this.height > member.getHeight()) {
			return 1;
		} else {
			return -1;
		}
	}
}
```

- the `compareTo()` method above can be implemented as follows
```java
@Override
public int compareTo(Member member) {
	return this.height - member.getHeight();
}
```

- With the Comparable interface, it is possible to sort the `sorted` method
```java
member.stream().sorted().forEach(m -> System.out.println(m));
```

- a stream does not sort the original list - *only the items in the stream are sorted*
- if a programmer wants to organize the original list, the `sort` method of the `Collections` class should be used
```java
Collections.sort(member);
member.stream().forEach(m -> System.out.println(m));
```

## Implementing Multiple Interfaces
- multiple interfaces are implemented by separating the implemented interfaces with commas (`public class ... implements *FirstInterface*, *SecondInterface*`)
- implementing multiple interface requires us to implement all of the methods for which implementations are required by the interfaces
```java
public interface Identifiable {
    String getId();
}
```

```java
public class Person implements Identifiable, Comparable<Person> {
    private String name;
    private String socialSecurityNumber;

    public Person(String name, String socialSecurityNumber) {
        this.name = name;
        this.socialSecurityNumber = socialSecurityNumber;
    }

    public String getName() {
        return this.name;
    }

    public String getSocialSecurityNumber() {
        return this.socialSecurityNumber;
    }

    @Override
    public String getId() {
        return getSocialSecurityNumber();
    }

    @Override
    public int compareTo(Person another) {
        return this.getId().compareTo(another.getId());
    }
}
```

## Sorting Method as a Lambda Expression
```java
Collection.sort(persons, (p1, p2) -> p1.getBirthYear() - p2.getBirthYear());
```

> When comparing strings, we can use the `compareTo` method provide by the String class

```java
persons.stream().sorted((p1, p2) -> {
	return p1.getName().compareTo(p2.getName());
}).forEach(p -> System.out.println(p.getName());
```

## Sorting by Multiple Criteria
- the `Comparator` class provides two essential methods for sorting: `comparing` and `thenComparing` the `comparing`method is passed the value to be compared first, and the `thenComparing` method is the next value to be compared (can be continuously chained).
```java
List<Film> films = new ArrayList<>();
films.add(new Film("A", 2000));
films.add(new Film("B", 1999));
films.add(new Film("C", 2001));
films.add(new Film("D", 2000));

for (Film e: films) {
    System.out.println(e);
}

Comparator<Film> comparator = Comparator
              .comparing(Film::getReleaseYear)
              .thenComparing(Film::getName);

Collections.sort(films, comparator);

for (Film e: films) {
    System.out.println(e);
}
```

### ==example== File `literacy.csv`, includes the literacy estimates for women and men over 15 years of age. Each line in the file `literacy.csv` is as follows: "theme, age group, gender, country, year, literacy percent. Below are the first five lines as an example.

```
Adult literacy rate, population 15+ years, female (%),United Republic of Tanzania,2015,76.08978
Adult literacy rate, population 15+ years, female (%),Zimbabwe,2015,85.28513
Adult literacy rate, population 15+ years, male (%),Honduras,2014,87.39595
Adult literacy rate, population 15+ years, male (%),Honduras,2015,88.32135
Adult literacy rate, population 15+ years, male (%),Angola,2014,82.15105

```

- Create a program that first reads the file `literacy.csv` and then prints the contents from the lowest to the highest ranked on the literacy rate. The output must be exactly as in the following example. The example shows the first five of the expected rows.

```
Niger (2015), female, 11.01572
Mali (2015), female, 22.19578
Guinea (2015), female, 22.87104
Afghanistan (2015), female, 23.87385
Central African Republic (2015), female, 24.35549
```

```java
// LiteracyComparison.java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.ArrayList;

public class LiteracyComparison {

    public static void main(String[] args) {
        ArrayList<Literacy> literacies = new ArrayList<>();

        try {
            Files.lines(Paths.get("literacy.csv"))
                    .map(row -> row.split(","))
                    .filter(parts -> parts.length >= 6)
                    .map(parts -> new Literacy(
                    parts[0],
                    parts[1],
                    parts[2],
                    parts[3],
                    Integer.valueOf(parts[4]),
                    Double.valueOf(parts[5])
            )).forEach(literacies::add);

            literacies.stream().sorted().forEach(System.out::println);

        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }

    }
}
```

```java
// Literacy.java

public class Literacy implements Comparable<Literacy> {

    private String theme;
    private String ageGroup;
    private String gender;
    private String country;
    private int year;
    private double literacyPercent;

    public Literacy(String theme, String ageGroup, String gender, String country, int year, double literacyPercent) {
        this.theme = theme;
        this.ageGroup = ageGroup;
        this.gender = gender;
        this.country = country;
        this.year = year;
        this.literacyPercent = literacyPercent;
    }

    @Override
    public String toString() {
        return this.country + " (" + this.year + ")," + this.gender.replace(" (%)", "") + ", " + this.literacyPercent;
    }

    @Override
    public int compareTo(Literacy literacy) {
        return Double.compare(this.literacyPercent,literacy.literacyPercent);
    }
}
```

---
## Measure Performance Time
```java
long start = System.nanoTime();
long end = System.nanoTime();
double durationInMilliseconds = 1.0 * (end - start) / 1000000;
```

---

**Review: Example: User Interface With Todo List Class**
```java
import java.util.Scanner;

public class Main {
    public static void main(String[] args) {
        TodoList list = new TodoList();
        Scanner scanner = new Scanner(System.in);
                
        UserInterface ui = new UserInterface(list, scanner);
        ui.start();
    }
}
```

```java
import java.util.ArrayList;

public class TodoList {
    private ArrayList<String> todos;

    public TodoList() { 
	    this.todos = new ArrayList<>(); 
	}

    public void add(String task) { 
	    this.todos.add(task); 
	}

    public void print() {
        for (int i = 0; i < todos.size(); i++) {
            System.out.println(i + 1 + ": " + todos.get(i));
        }
    }

    public void remove(int number) { 
	    this.todos.remove(number - 1); 
	}
}
```

```java
import java.util.Scanner;

public class UserInterface {
    private TodoList list;
    private Scanner scanner;

    public UserInterface(TodoList list, Scanner scanner) {
        this.list = list;
        this.scanner = scanner;
    }

    public void start() {
        while (true) {
            System.out.println("Command: ");
            String command = scanner.nextLine();

            if (command.equals("stop")) {
                break;
            }

            if (command.equals("add")) {
                this.add();
            }

            if (command.equals("list")) {
                this.list.print();
            }

            if (command.equals("remove")) {
                this.remove();
            }
        }
    }

    public void add() {
        System.out.println("To add: ");
        String task = scanner.nextLine();
        list.add(task);
    }

    public void remove() {
        System.out.println("Which one is removed? ");
        int number = Integer.valueOf(scanner.nextLine());
        list.remove(number);
    }
}

```

