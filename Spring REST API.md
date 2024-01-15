### CORS
- `WebMvcConfigurer.class` > `addCorsMappings`
```
@Bean
public WebMvcConfigurer corsConfigurer() {
	return new WebMvcConfigurer() {
		public void addCorsMappings(CorsRegistry registry) {
			registry
					.addMapping("/**")
					.allowedMethods("*")
					.allowedOrigins("http://localhost:3000");
		}
	};
}
```

### Get Mapping

```
// TodoService.java
@Service
public class TodoService {
	private static List<Todo> todos = new ArrayList<>();

    public Todo findById(int id) {
        Predicate<? super Todo> predicate = todo -> todo.getId() == id;
        Todo todo = todos.stream().filter(predicate).findFirst().get();
        return todo;
    }
}
```

```
@RestController
public class TodoResource {
    private TodoService todoService;

    public TodoResource(TodoService todoService) {
        this.todoService = todoService;
    }

    @GetMapping("/users/{username}/todos/{id}")
    public Todo retrieveTodo(@PathVariable String username, @PathVariable int id) {
        return todoService.findById(id);
    }
}
```

### DELETE Mapping
```
// TodoService.java
@Service
public class TodoService {
	private static List<Todo> todos = new ArrayList<>();

    public void deleteById(int id) {
        Predicate<? super Todo> predicate = todo -> todo.getId() == id;
        todos.removeIf(predicate);
    }
}
```

```
@RestController
public class TodoResource {
    private TodoService todoService;

    public TodoResource(TodoService todoService) {
        this.todoService = todoService;
    }

    @DeleteMapping("/users/{username}/todos/{id}")
    public ResponseEntity<Void> deleteTodo(
		@PathVariable String username, @PathVariable int id
	) {
        todoService.deleteById(id);
        return ResponseEntity.noContent().build();
    }
}
```

### PUT Mapping
```
// TodoService.java
@Service
public class TodoService {
	private static List<Todo> todos = new ArrayList<>();

    public void updateTodo(Todo todo) {
        deleteById(todo.getId());
        todos.add(todo);
    }

```

```
@RestController
public class TodoResource {
    private TodoService todoService;

    public TodoResource(TodoService todoService) {
        this.todoService = todoService;
    }

    @PutMapping("/users/{username}/todos/{id}")
    public Todo updateTodo(
	    @PathVariable String username, @PathVariable int id, @RequestBody Todo todo
    ) {
        todoService.updateTodo(todo);
        return todo;
    }
}
```

### POST Mapping
```
// TodoService.java
@Service
public class TodoService {
	private static List<Todo> todos = new ArrayList<>();

    public Todo addTodo(
		    String username, String description, LocalDate targetDate, boolean done
	    ) {
        Todo todo = new Todo(++todosCount, username, description, targetDate, done);
        todos.add(todo);
        return todo;
    }

```

```
@RestController
public class TodoResource {
    private TodoService todoService;

    public TodoResource(TodoService todoService) {
        this.todoService = todoService;
    }

    @PostMapping("/users/{username}/todos")
    public Todo createTodo(@PathVariable String username, @RequestBody Todo todo) {
        Todo createdTodo = todoService.addTodo(
	        username, todo.getDescription(), todo.getTargetDate(), todo.isDone()
        );
        return createdTodo;
    }
}

