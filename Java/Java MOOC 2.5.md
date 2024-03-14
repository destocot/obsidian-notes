**TABLE OF CONTENTS**
- [[#StringBuilder]]
- [[#Regular Expressions]]
- [[#Enumerated Type - Enum]]
- [[#Iterator]]
- [[#Class Diagrams]]

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

