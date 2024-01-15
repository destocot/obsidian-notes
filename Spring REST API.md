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
@RestController
public class TodoResource {
    private TodoService todoService;

    public TodoResource(TodoService todoService) {
        this.todoService = todoService;
    }

    @GetMapping("/users/{username}/todos")
    public List<Todo> retrieveTodos(@PathVariable String username) {
        return todoService.findByUsername(username);
    }
}
```

