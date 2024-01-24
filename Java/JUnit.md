## Testing
- **Option 1**: Deploy the complete application and test
	- This is called **System Testing** or **Integration Testing**
- **Option 2**: Test specific units of application code independently
	- Examples: A specific method or group of methods
	- This is called **Unit Testing**

```java
public class Methods {
	public int squareNumber(int num) {
		return num * num;
	}
	
	public void method2() {}
	public void method3() {}
}
```

```java
squareNumber(5); // 25
squareNumber(25); // 625
```

### **Advantages of Unit Testing**
- Finds bugs early (run under CI)
- Easy to fix bugs

# Most Popular Java Testing Frameworks

## JUnit - Unit testing framework

- absence of failure is success

```java
public class MyMath {
    public int calculateSum(int[] numbers) {
        int sum = 0;
        for (int number: numbers) { sum += number; }
        return sum;
    }
}

```

> @Test annotation
```java
class MyMathTest {
    private final MyMath math = new MyMath();

    @Test
    void calculateSum_ThreeMemberArray() {
        int actual = math.calculateSum(new int[]{ 1, 2, 3 });
        assertEquals(6, actual);
    }

    @Test
    void calculateSum_ZeroLengthArray() {
        int actual = math.calculateSum(new int[]{ });
        assertEquals(0, actual);
    }
}
```

#### Assert methods
```java
assertTrue(<optional | "error message">,<boolean>);
assertFalse(<optional | "error message">,<boolean>);
assertEquals(<expected>, <actual>, <optional | "error message">);
assertNull(<actual>);
assertNotNull(<actual>);
assertArrayEquals(<expected array>, <actual array>);
```

> JUnit does **not** guarantee the same execution order as tests are written.
#### Annotations

> @BeforeEach annotation (setup before each test)
```java
public class MyBeforeAfterTest {  
  
    @BeforeEach  
    void beforeEach() {  
        System.out.println("Before Each");  
    }

	/* ... tests ... */
}
```

> @AfterEach annotation (cleanup after each test)
```java
public class MyBeforeAfterTest {  

	/* ... tests ... */
  
	@AfterEach  
	void afterEach() {  
	    System.out.println("After Each");  
	}
}
```

- @BeforeAll and @AfterAll must be **static** because they are class level methods since they run before all other tests.

> @BeforeAll (common setup for all tests)
```java
public class MyBeforeAfterTest {  
  
    @BeforeAll
    static void beforeAll() {
        System.out.println("Before All");
    }

	/* ... tests ... */
}
```

> @BeforeAll (common cleanup for all tests)
```java
public class MyBeforeAfterTest {  

	/* ... tests ... */
  
    @AfterAll
    static void afterAll() {
        System.out.println("After All");
    }
}
```
## Appendix (packages)
```java
import static org.junit.jupiter.api.Assertions.*;
(.assertEquals | .assertTrue | .assertFalse | .assertNull | .assertNotNull | .assertArrayEquals)

import org.junit.jupiter.api.*;
(.Test | .BeforeEach | .AfterEach | .BeforeAll | .AfterAll)
```

---
## Mockito - Mocking framework

