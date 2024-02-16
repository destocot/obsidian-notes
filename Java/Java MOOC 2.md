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