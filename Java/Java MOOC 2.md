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

