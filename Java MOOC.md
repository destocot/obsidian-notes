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

