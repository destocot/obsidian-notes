## Unit Testing
- unit testing refers to the testing of individual components in the source code, such as classes and their provided methods.
- The most common unit testing library in **Java** is **JUnit**, which is also supported by almost all programming environments.

==example== Writing **JUnit** tests for a Calculator class
- the subtract method is purposely written to have a mistaken in it (the method should deduct from the value, but it currently adds to it.)
```java
public class Calculator {
    private int value;

    public Calculator() {
        this.value = 0;
    }

    public void add(int number) {
        this.value = this.value + number;
    }
	
    public void subtract(int number) {
        this.value = this.value + number;
    }

    public int getValue() {
        return this.value;
    }
}
```

- Unit test writing begins by creating a test class
	- this test class is created under the Test-Packages folder
- The string `Test` at the end of the name tells the programming environment that this a test class.
	- without the string Test, the tests in the class will NOT be executed

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {
	@Test
	public void calculatorInitialValueZero() {
		Calculator calculator = new Calculator();
		assertEquals(0, calculator.getValue());
	}

    @Test
    public void valueFiveWhenFiveAdded() {
        Calculator calculator = new Calculator();
        calculator.add(5);
        assertEquals(5, calculator.getValue());
    }

    @Test
    public void valueMinusTwoWhenTwoSubstracted() {
        Calculator calculator = new Calculator();
        calculator.subtract(2);
        assertEquals(-2, calculator.getValue());
    }
}
```
- each test method should have an "annotation" `@Test`
- the `assertEquals` method provided by the JUnit test framework is used to compare the expected value (0) with the value returned by the calculator `calculator.getValue()`

> To run the tests, select the project with the right-mouse button and click Test.

- the tests produce the following output:
```bash
Testsuite: CalculatorTest
Tests run: 3, Failures: 1, Errors: 0, Skipped: 0, Time elapsed: 0.059 sec

Testcase: valueMinusTwoWhenTwoSubstracted(CalculatorTest): FAILED
expected:<-2> but was:<2>
junit.framework.AssertionFailedError: expected:<-2> but was:<2>
at CalculatorTest.valueMinusTwoWhenTwoSubstracted(CalculatorTest.java:25)

Test CalculatorTest FAILED
test-report:
test:
BUILD SUCCESSFUL (total time: 0 seconds)
```

- this output tells us that three tests were executed, one of them failed.
- the test informs us which line the error occurred (25)
- the test informs us of the expected (-2) and the actual (2) values

- fixing the error
```java
// ...
public void subtract(int number) {
    this.value -= number;
}
// ...
```
- re-run the tests
```bash
Testsuite: CalculatorTest
Tests run: 3, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.056 sec

test-report:
test:
BUILD SUCCESSFUL (total time: 0 seconds)
```

> Unit testing tends to be extremely complicated if the whole application has been written in "Main". To make testing easier, the app should be split into small parts, each having a clear responsibility.

## Test-driven software development
- a process that's based on constructing a piece of software in small iterations.
- consists of five steps that are repeated until the functionality of the program is complete

1. Create a test that tests some feature that will be added to the program.
2. Run the tests and check if the tests pass. A new test should not pass and only test functionality that hasn't yet been implemented.
3. Develop the program so that it has the functionality required to pass the test.
4. Perform the tests. If the tests fail, there is a likely to be an error in the functionality written.
5. Refactor. As the size of the program increases, its internal structure is adjusted as needed (tests are not modified, but are instead used to verify the correctness of the changes made to the program's internal structure)





==example== Passing string to the Scanner object
```java
String input = "one\n" + "two\n"  +  "three\n" + "four\n" +
                "five\n" + "one\n"  +  "six\n";

Scanner reader = new Scanner(input);
ArrayList<String> read = new ArrayList<>();

while (true) {
    System.out.println("Enter an input: ");
    String line = reader.nextLine();
    if (read.contains(line)) {
        break;
    }

    read.add(line);
}
reader.close();
System.out.println("Thank you!");

if (read.contains("six")) {
    System.out.println("A value that should not have been added to the group was added to it.");
}
```


