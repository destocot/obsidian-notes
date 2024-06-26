**TABLE OF CONTENTS**
- [[#StringBuilder]]
- [[#Regular Expressions]]
- [[#Enumerated Type - Enum]]
- [[#Iterator]]
- [[#Class Diagrams]]
- [[#Packages]]
- [[#Exceptions]]
- [[#Processing Files]]
- [[#Type Parameters]]
- [[#ArrayList and hash table]]
	- [[#Lists]]
	- [[#Hash map]]
- [[#Randomness]]
- [[#Multidimensional data]]

- [[#Maven and third-party libraries]]

---
# StringBuilder
- Java's ready-made StringBuilder class provides a way to concatenate strings without the need to create them.
```java
StringBuilder numbers = new StringBuilder();
for (int i = 1; i < 5; i++) {
	numbers.append(i);
}
System.out.println(numbers.toString());
```

> Using StringBuilder is more efficient than creating strings with the `+` operator.

# Regular Expressions
- Regular expressions are used, among other things, to verify the correctness of strings

- ==example== a student number should begin with "01" followed by 7 digits between 0-9.
```java
System.out.println("Provide a student number: ");
String number = scanner.nextLine();

if (number.match("01[0-9]{7}")) { /*...*/ }
```

## Common Characters Used in Regular Expressions

### Alternation (Vertical Line)
- a vertical line indicates that parts of a regular expressions are optional
- returns `true` if the string matches any one of the specified group of alternatives
```java
"00".matches("00|111|0000") // true
"1111".matches("00|111|0000") // false
```

### Affecting Part of a String (Parentheses)
- you can use parentheses to determine which part of a regular expression is affected by the rules inside the parentheses.
```java
"00001".matches("00000|00001"); // true
"00001".matches("0000(0|1)"); // true
```

```java
"car".matches("car(|s|)"); // true;
"cars".matches("car(|s|)"); // true;
```

### Quantifiers
- a  particular sub-string is repeated in a string

- the quantifier `*` matches 0+ repetitions of the group "lo"
```java
"trolololololo".matches("trolo(lo)*"); // true
```

- the quantifier `+` matches 1+ repetitions of the group "lo"
```java
"trolololololo".matches("tro(lo)+"); // true
"nananananananana Batmaan!".matches("(na)+ Batmaan!"); // true
```

- the quantifier `?` matches 0 or 1 times
```java
String string = "You have to accidentally the whole meme";

string.matches("You have to accidentally (delete )?the whole meme"); // true
```

- the quantifier `{a}` repeats `a` times
```java
"1010".matches("(10){2}"); // true
```

- the quantifier `{a, b}` repeats `a` - `b` times
```java
"1".matches("1{2,4}"); // false
"11".matches("1{2,4}"); // true
```

- the quantifier `{a,}` repeats `a` times
```java
"11111".matches("1{2,}"); // true
```

### Character Classes (Square Brackets)
- a character class can be used to specify a set of characters in a compact way
- `[145]` means `(1|4|5)`
	- `"1".matches("[145]"); // true`
- `[2-36-9]` means `(2|3|6|7|8|9)
	- `"7".matches("[2-36-9]"); // true`
- `[a-c]*` means the string to contain only `a`, `b`, or `c`
	- `"abccba".matches("[a-c]*"); // true`

==examples==
```java
public class Checker {    
	public boolean isDayOfWeek(String string) {
        return string.matches("mon|tue|wed|thu|fri|sat|sun");
    }
 
    public boolean allVowels(String string) {
        return string.matches("[aeiou]*");
    }
 
    public boolean timeOfDay(String string) {
        return string.matches("([01][0-9]|[0-2][0-3]):[0-5][0-9]:[0-5][0-9]");
    }
}
```

# Enumerated Type - Enum
- if we know the possible values of a variable in advance, we can use a class of type `enum,` i.e. *enumerated type* to represent the values
```java
public enum Suit {
	DIAMOND, SPADE, CLUB, HEART
}
```

```java
public class Card {

    private int value;
    private Suit suit;

    public Card(int value, Suit suit) {
        this.value = value;
        this.suit = suit;
    }

    @Override
    public String toString() {
        return suit + " " + value;
    }

    public Suit getSuit() {
        return suit;
    }

    public int getValue() {
        return value;
    }
}
```

```java
Card first = new Card(10, Suit.HEART);

System.out.println(first);

if (first.getSuit() == Suit.SPADE) {
    System.out.println("is a spade");
} else {
    System.out.println("is not a spade");
}
```

> each enum field gets a unique number code, and they can be compared using the equals sign. The numeric identifier of an enum field can be found with `odinal()`

```java
System.out.println(Suit.DIAMOND.ordinal()); // 0
System.out.println(Suit.HEART.ordinal()); // 3
```

#### Object References In Enums
```java
public enum Color {
    // constructor parameters are defined as
    // the constants are enumerated
    RED("#FF0000"),
    GREEN("#00FF00"),
    BLUE("#0000FF");

    private String code;        // object reference variable

    private Color(String code) { // constructor
        this.code = code;
    }

    public String getCode() {
        return this.code;
    }
}
```

```java
System.out.println(Color.GREEN.getCode()); // #00FF00
```

# Iterator
- ArrayList and other "object containers" that implement the `Collection` interface implement the `Iterable` interface, and they can also be iterated over with the help of an `iterator`.
```java
public void print() {
    Iterator<Card> iterator = cards.iterator();

    while (iterator.hasNext()) {
        Card nextInLine = iterator.next();
        System.out.println(nextInLine);
    }
}
```

- removing from the list the element returned by the previous next-method call
```java
public void print() {
    Iterator<Card> iterator = cards.iterator();

    while (iterator.hasNext()) {
		if (iterator.next().getValue() < value) {
			iterator.remove();
		}
	}
}
```

---

# Class Diagrams
- a class diagram is a diagram used in designing and modeling software to describe classes and their relationships

```java
// Person.java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.name = initialName;
        this.age = 0;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " +   this.age + " years");
    }

    public String getName() {
        return this.name;
    }
}
```

- class diagram

| Person                                                                        |
| ----------------------------------------------------------------------------- |
| - name:String<br>- age:int                                                    |
| +Person(`initialName`:String)<br>+`printPerson()`:void<br>+`getName()`:String |
- methods are written with +/- (depending on the visibility of the method)
- constructors are listed first and then all class methods
- a class diagram describes the structure of an object but not its functionality.
#### Connections between classes

```java
// Book.java
public class Book {
    private String name;
    private String publisher;
    private ArrayList<Person> authors;

    // constructor

    public ArrayList<Person> getAuthors() {
        return this.authors;
    }

    public void addAuthor(Person author) {
        this.authors.add(author);
    }
}
```

| Book                                                     |                   | Person                                                                        |
| -------------------------------------------------------- | ----------------- | ----------------------------------------------------------------------------- |
| -name:String<br>-publisher:String                        | =====><br>author* | -name:String<br>-age:int                                                      |
| +`getAuthors()`:ArrayList<br>+`addAuthor(author:Person)` |                   | +Person(`initialName`:String)<br>+`printPerson()`:void<br>+`getName()`:String |
- the arrow shows the direction of the connection
- we can also add a label to the arrow to describe the connection (Book knows about the person but Person doesn't know about the Book)
- by adding a star to the end of the arrow tells us that a book can have between 0 and unlimited number of authors.

- if there is no arrowhead in a connection, both classes know about each other
```java
public class Book {
	private ArrayList<Person> authors;
}
```

```java
public class Person {
	private Book boo;
}
```

#### Describing inheritance
- inheritance is describe by an arrow with a triangle head

| Part                                                      |
| --------------------------------------------------------- |
| -id:String<br>-manufacturer:String<br>-description:String |
| ^<br>\|                                                   |
| **Engine**                                                |
| -type:String                                              |
- Engine inherits the class Part

- Inheritance of abstract classes is described almost the same way as regular classes. However we add the description `<<abstract>>` above the name of the class.

| `<<abstract>>`<br>Box                                                                              |                |                                                                                |
| -------------------------------------------------------------------------------------------------- | -------------- | ------------------------------------------------------------------------------ |
|                                                                                                    |                |                                                                                |
| + add(item:item): void<br>+ add(items:Collection): void<br>+ `isInBox`(item:Item): boolean         |                |                                                                                |
| ^<br>\|                                                                                            |                |                                                                                |
| **BoxWithMaximumWeight**                                                                           |                | **Item**                                                                       |
| -maximumWeight: int                                                                                | ====><br>item* | + name: String<br>+ weight: int                                                |
| + `BoxWithMaximuWeight(capacity:int)`<br>+ add(item:Item): void<br>+ `isInBox(item:Item)`: boolean |                | + Item(name:String, weight:int)<br>+ Item(name:String)<br>+ `getWeight()`: int |
#### Describing interfaces
- in class diagrams, interfaces are written `<<interface>> NameOfTheInterface`

| Book | ====> | `<<interface>>`<br>Readable |
| ---- | ----- | --------------------------- |

# Packages
- packages are directories in which the source code files are organised.

- the package of a class is noted at the beginning of the source code file with the statement `package *name-of-package`
```java
package library;

public class Program {

    public static void main(String[] args) {
        System.out.println("Hello packageworld!");
    }
}
```

- every package, including the default package, may contain other packages
- a class can access classes inside a package by using the `import statement`
```java
package library;

import library.domain.Book;

public class Program {

    public static void main(String[] args) {
        Book book = new Book("the ABCs of packages!");
        System.out.println("Hello packageworld: " + book.getName());
    }
}
```

#### Packages and access modifiers
- if the access modifier is missing, the methods or variables are only visible inside the same packages.
```java
package library.ui;

public class UserInterface {

	/*...*/

	void printTitle() {
		/*...*/
	}
}
```

- classes inside the same package (`library.ui`) can use the method `printTitle`
```java
package library.io;

public class Main {
	UserInterface ui = new UserInterface(/*...*/);
	ui.printTitle(); // works!
}
```

- if the class is in a different package, the method `printTitle` cannot be called.
```java
package library;

import library.ui.UserInterface;

public class Main {
	UserInterface ui = new UserInterface(/*...*/);
	ui.printTitle(); // doesn't work!
}
```

# Exceptions

#### Handling Exceptions
- we use the `try {} catch (Exception e) {}` block structure to handle exceptions.
```java
public int readNumber(Scanner reader) {
	while (true) {
		System.out.print("Give a number: ");
		
		try {
			// code which possibly throws an exception
			int readNumber = Integer.parseInt(reader.nextLine());
			return readNumber;
		} catch (Exception e) {
			// code block executed if an exception is thrown
			System.out.println(" User input was not a number.");
		}
	}
}
```

- output
```
Give a number: no!
User input was not a number.
Give a number: Matt has moss in his head
User input was not a number.
Give a number: 43
```

#### Exceptions and resources
- there is separate exception handling for reading operating system resources such as files
	- try-with-resources exception handling
	- the program closes the used resources automatically
```java
ArrayList<String> lines =  new ArrayList<>();

// create a Scanner object for reading files
try (Scanner reader = new Scanner(new File("file.txt"))) {

	// read all lines from a file
	while (reader.hasNextLine()) {
		lines.add(reader.nextLine());
	}
} catch (Exception e) {
	System.out.println("Error: " + e.getMessage());
}

// do something with the lines
```

#### Shifting the responsibility
- we can leave the exception unhandled and shift the responsibility for handling it to whomever calls the method
```java
public List<String> readLines(String fileName) throws Exception {
	ArrayList<String> lines = new ArrayList<>();
	Files.lines(Paths.get(fileName)).forEach(line -> lines.add(line));
	return lines;
}
```
- now the method calling the `readLines` method has to either handle the exception in a `try-catch` block or shift the responsibility yet forward (sometimes the responsibility of handling exceptions is avoided until the end, and event eh `main` method can throw an exception to the caller)

```java
public class MainProgram {
	public static void main(String[] args) throws Exception {
		/*...*/
	}
}
```

#### Throwing exceptions
- the `throw` command throws an exception
```java
if (grade < 0 || grade > 5) {
	throw new IllegalArgumentException("Grade must be between 0 and 5.");
}
```

#### Exceptions and Interfaces
- an interface can have methods which throw an exception
```java
public interface FileServer {
	String load(String fileName) throws Exception;
	void save(String fileName, String textToSave) throws Exception;
}
```
- if an interface declares a `throws Exception` attribute to a method, the class implementing this interface must also have this attribute (however, the class does not have to throw an error)

# Processing Files

#### Writing data to files (`PrintWriter` class)
```java
PrintWriter writer = new PrintWriter("file.txt");
writer.println("Hello file!"); 
writer.println("More text");
write.print("And a little extra"):
writer.close();
```

- the `PrintWriter` class might throw an exception
```java
public class Storer {
	public void writeToFile(String fileName, String text) throws Exception {
		PrintWriter writer = new PrintWriter(fileName);
		writer.println(text);
		writer.close();
	}
}
```

```java
public static void main(String[] args) throws Exception {
    Storer storer = new Storer();
    storer.writeToFile("diary.txt", "Dear diary, today was a good day.");
}
```

> The `PrintWriter` class will erase previous text if the file already exists. In order to add new text to an existing content, check out the `FileWriter` class.

==example== Savable Dictionary
```java
package dictionary;
 
import java.io.IOException;
import java.io.PrintWriter;
import java.nio.file.Files;
import java.util.HashMap;
import java.nio.file.Paths;
import java.util.ArrayList;
 
public class SaveableDictionary {
 
    private String file;
    private HashMap<String, String> words;
 
    public SaveableDictionary() {
        this.words = new HashMap<>();
    }
	// Initializes the dictionary with default values
    public SaveableDictionary(String file) {
        this();
        this.file = file;
    }
    ```
- loads file, parses lines, and adds each entry to dictionary
```java
    public boolean load() {
        try {
            Files.lines(Paths.get(this.file))
                    .map(row -> row.split(":"))
                    .forEach(parts -> {
                        this.words.put(parts[0], parts[1]);
                        this.words.put(parts[1], parts[0]);
                    });
            return true;
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
            return false;
        }
    }
    ```
- saves dictionary to file
```java
    public boolean save() {
        try {
            PrintWriter writer = new PrintWriter(file);
            ArrayList<String> alreadySaved = new ArrayList<>();
 
            this.words.keySet().stream().forEach(word -> {
                if (alreadySaved.contains(word)) {
                    return;
                }
 
                String translation = this.words.get(word);
                writer.println(word + ":" + translation);
 
                alreadySaved.add(word);
                alreadySaved.add(translation);
            });
            writer.close();
            return true;
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
            return false;
        }
    }
    ```
- adds <Word, Translation> and <Translation, Word> entries to HashMap
```java
    public void add(String words, String translation) {
        if (this.words.containsKey(words)) {
            return;
        }
 
        this.words.put(words, translation);
        this.words.put(translation, words);
    }
```
- returns translation of word
```java
    public String translate(String word) {
        return this.words.get(word);
    }
```
- removes <Word, Translation> and <Translation, Word> entries to HashMap
```java
    public void delete(String word) {
        String translation = this.words.get(word);
        words.remove(word);
        words.remove(translation);
    }
```

```
}
```

---
# Type Parameters
- *Generics* relates to how classes that store objects can store objects of a freely chosen type.
```java
public class Locker<T> {
	private T element;

	public void setValue(T element) {
		this.element = element;
	}

	public T getValue() {
		return element;
	}
}
```

```java
Locker<String> string = new Locker<>();
string.setValue(":)");

System.out.println(string.getValue());
```

- creating generic interfaces is very similar to creating generic classes
```java
public interface List<T> {
	void add(T value);
	T get(int index);
	T remove(int index);
}
```

- two ways of implementing a generic interface

1. decide the type parameter in the definition of the class
```java
public class MovieList implements List<Movie> {
	/*...*/
}
```

2. define the implementing class with a type parameter as well.
```java
public class GeneralList<T> implements List<T> {
	/*...*/
}
```

# ArrayList and hash table

#### a brief recap of arrays
- an array is an object that contains a limited number of places for values
- the size of an array is always predetermined and cannot be changed later
	- `int[] numbers = new int[3];`
## Lists
- Java ArrayList uses an array
- The type of the elements in the defined by the type parameter given to the ArrayList
	- `ArrayList<String> strings = new ArrayList<>();`
- Relevant methods (among many others):
	- add: `strings.add("Hello");`
	- remove: `strings.remove("Hello");`
	- contains: `strings.contains("Hello");`
	- get: `strings.get(0);`

#### Creating a new list
```java
public class List<Type> {
	private Type[] values;
	private int firstFreeIndex;

	public List() {
		this.values = (Type[]) new Object[10];
		this.firstFreeIndex = 0;
	}

	// adding values to the list
	public void add(Type value) {
		if (this.firstFreeIndex == this.values.length) {
			grow();
		}
		this.values[this.firstFreeIndex] = value;
		this.firstFreeIndex++;
	}

	// increasing the size (1.5x) of the list
	private void grow() {
		int newSize = this.values.length + this.values.length / 2;
		Type[] newValues = (Type[]) new Object[new Size];
		for (int i = 0; i < this.values.length; i++) {
			newValues[i] = this.values[i];
		}

		this.values = newValues;
	}

	// Check whethere the list contains a value or not
	// Each object has the method `public boolean equals(Object object)`,
	// which can be used to check equality
	public boolean contains(Type value) {
		return indexOfValue(value) >= 0;
	}

	// removing a value from the list
	public void remove(Type value) {
		int indexOfValue = indexOfValue(value);
		if (indexOfValue < 0) {
			return; // not found
		}

		moveToTheLeft(indexOfValue);
		this.firstFreeIndex--;
	}

	/* FUTURE IMPROVEMENT: decrease the size of the List */

	// searches for the index of the given value
	punlic int indexOfValue(Type value) {
		for (int i = 0; i < this.firstFreeIndex; i++) {
			if (this.values[i].equals(value)) {
				return i;
			}
		}
		return -1;
	}

	// moves values from the given index one place to the left
	private void moveToTheLeft(int fromIndex) {
		for (int i = fromIndex; i < this.firstFreeIndex - 1; i++) {
			this.values[i] = this.values[i + 1];
		}
	}

	// searching from an index
	public Type value(int index) {
		if (index < 0 || index >= this.firstFreeIndex) {
			throw new ArrayIndexOutOfBoundsException(
				"Index " + index + " outside of [0, " + this.firstFreeIndex + "]"
			);
		}
		return this.values[index];
	}

	// size of the list
	public int size() {
		return this.firstFreeIndex;
	}
}
```

## Hash map
- Hash map is implemented as an array, in which every element includes a list
- The list contains (key, value) pairs - each key can appear at most once in the hash map

#### Key-value pair
```java
public class Pair<K, V> {
	private K key;
	private V value;

	public Pair(K key, V value) {
		this.key = key;
		this.value = value;
	}

	public K get Key() {
		return key;
	}

	public V getValue() {
		return value;
	}

	public void setValue(V value) {
		this.value = value;
	}
}
```

#### creating a hash map
```java
public class HashMap<K, V> {
	private List<Pair<K, V>>[] values;
	private int firstFreeIndex;

	public HashMap() {
		this.values = new List[32];
		this.firstFreeIndex = 0;
	}

	public V get(K key) {
		int hashValue = Math.abs(key.hashCode() % this.values.length);
		if (this.values[hashValue] == null) {
			return null;
		}

		List<Pair<K, V>> valuesAtIndex = this.values[hashValue];

		for (int i = 0; i < valuesAtIndex.size(); i++) {
			if (valuesAtIndex.value(i).getKey().equals(key)) {
				return valuesAtIndex.value(i).getValue();
			}
		}
		return null;
	}

	public void add(K key, V value) {
		List<Pair<K,V>> valuesAtIndex = getListBasedOnKey(key);
		int index = getIndexOfKey(valuesAtIndex, key);

		if (index < 0) {
			valuesAtIndex.add(new Pair<>(key, value));
			this.firstFreeIndex++;
		} else {
			valuesAtIndex.value(index).setValue(value);
		}

		if (1.0 * this.firstFreeIndex / this.values.length > 0.75) {
			grow();
		}
	}

	// finding the list related to the key
	private List<Pair<K, V>> getListBasedOnKey(K key) {
	    int hashValue = Math.abs(key.hashCode() % values.length);
	    
	    if (values[hashValue] == null) {
	        values[hashValue] = new List<>();
	    }
	    return values[hashValue]
	}

	// finding the key on that list
	private int getIndexOfKey(List<Pair<K, V>> myList, K key) {
	    for (int i = 0; i < myList.size(); i++) {
	        if (myList.value(i).getKey().equals(key)) {
	            return i;
	        }
	    }
	    return -1;
	}

	// grow size of the internal array (2x) of the hash map
	private void grow() {
		List<Pair<K,V>>[] newArray = new List[this.values.length * 2];

		// copy the values of the old array into the new one
		for (int i = 0; i < this.values.length; i++) {
			copy(newArray, i);
		}

		this.values = newValues;
	}

	private void copy(List<Pair<K, V>>[] newArray, int fromIdx) {
		for (int i = 0; i < this.values[fromIdx].size(); i++) {
			Pair<K, V> value = this.values[fromIdx].value(i);

			int hashValue = Math.abs(
				value.getKey().hashCode() % newArray.length
			);

			if(newArray[hashValue] == null) {
				newArray[hashValue] = new List<>();
			}

			newArray[hashValue].add(value);
		}
	}

	public V remove(K key) {
	    List<Pair<K, V>> valuesAtIndex = getListBasedOnKey(key);
		if (valuesAtIndex.size() == 0) {
			return null;
		}
		
		int index = getIndexOfKey(valuesAtIndex, key);
		if (index < 0) {
			return null;
		}

		
		Pair<K, V> pair = valuesAtIndex.value(index);
		valuesAtIndex.remove(pair);
		return pair.getValue();
	}
}
```

- **Since our `HashMap.get(K key)` method returns null if the key was not found, we have to use the `Integer` wrapper type to deal with it. We cannot use `int` because `int` is a primitive type and cannot be assigned `null`**
```java
HashMap<String, Integer> myMap = new HashMap<>();
 
myMap.add("Alice", 25);
myMap.remove("Alice");

Integer aliceAge = myMap.get("Alice");
```



# Randomness
- Java offers a ready-made `Random` class for creating random numbers
```java
import java.util.Random;

public static void main(String[] args) {
	Random ladyLuck = new Random();
	int randomNumber = ladyLuck.nextInt(10); // [0, 10)
	System.out.println(randomNumber);
}
```

```java
	double randomDouble = this.random.nextDouble(); // [0.0, 1.0);
	int randomGaussian = (int) (4 * this.randomGaussian() - 3);
```

> `randomGaussian()` returns a value with a mean of 0.0 and standard deviation of 1.0. The (4*) spreads the values out more but still with a mean of 0.0. The (-3) shifts the distribution to the left so the mean will be -3.0, having more negative numbers in the distribution.

#### On randomness of numbers
- random numbers used by computer programs are typically pseudo-random. They seem like random numbers, but in reality they follow some algorithmically created series of numbers.

# Multidimensional data
- we can create multidimensional arrays
- we will need the indexes of a value in each dimensions to access the value
```java
int rows = 2;
int columns = 3;
int[][] twoDimensionalArray = new int[rows][columns];
```

> the default value of variables type int is 0.


```java
/* PART 13 of Java MOOC is the JavaFx Section */
```

---

# Maven and third-party libraries
- Maven - used for executing and managing programs
- Every project has a file called `pom.xml` located at its root
- The source code is located in a directory called `src`
	- Typically has two sub-directories `main` and `test`

- Search engine for libraries - https://mvnrepository.com/ 
- `H2` is a popular database for getting started

