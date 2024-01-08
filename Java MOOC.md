# Lists

## import the list to make it available for the program
```
import java.util.ArrayList;
```

## create a new list
```
ArrayList<Type> list = new ArrayList<>();
```

> once a list has been created, ArrayList assumes that all variables contained in it are reference type ( Integer, Double, Boolean, String)

## add to a list
```
list.add(<value>);
```

## retrieve from list
```
list.get(<index>)
```

## size of a list
```
list.size();
```

## ==example== iterating over a list with a for-each loop
```
// if you don't need to keep track of the indexes
ArrayList<String> teachers = new ArrayList<>();

for (String techer: teachers) {
	System.out.println(teacher);
}
```

## remove from a list
```
list.remove(<index>);

list.remove(<value>);
```

> to remove an integer directly use the `Integer.valueOf(<number>)` method

## check for existence of a value in a list
```
list.contains(<value>); // returns a boolean
```

