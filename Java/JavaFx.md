- [[#Graphical user interfaces]]
	- [[#UI components and their layout]]
- [[#Event handling]]
- [[#Application's launch parameters]]
- [[#Multiple views]]

- [[#Data visualization]]


# Graphical user interfaces
- a library called JavaFX is used to create graphical user interfaces

- creating a simple window using JavaFX
```java
package application;

import javafx.application.Application;
import javafx.scene.Scene;
import javafx.scene.control.Button;
import javafx.scene.layout.FlowPane;
import javafx.stage.Stage;

public class JavaFxApplication extends Application {

    @Override
    public void start(Stage window) {
        Button button = new Button("This is a button");

        FlowPane componentGroup = new FlowPane();
        componentGroup.getChildren().add(button);

        Scene scene = new Scene(componentGroup);

        window.setScene(scene);
        window.show();
    }

    public static void main(String[] args) {
        launch(JavaFxApplication.class);
    }
}
```

#### Structure of a User Interface

**three essential parts**
1. the stage object  behaves as the program's window
2. a scene is set for a stage object that represents a scene within the window
3. the scene object, contains an object responsible for arranging the components belong to the scene

**UI structure**
-  the window contains a Scene object.
- the Scene object contains the object responsible for the layout of the user-interface components
- the object responsible for the component layout can contain both UI components and objects responsible for UI component layouts.

## UI components and their layout

**Label**
- text can be displayed using the Label class
```java
Label label = new Label("Text element");
```

**Button**
```java
Button button = new Button("This is a button");
```

**TextField**
```java
TextField textField = new TextField();
```

**TextArea**
```java
TextArea textArea = new TextArea();
```

#### UI Component Layout

**FlowPane**
- components that are added to the FlowPane are placed side-by-side
```java
Button button = new Button("This is a button");
FlowPane componentGroup = new FlowPane();
componentGroup.getChildren().add(button);
```

**BorderPane**
- components in the BorderPane can be laid out in five different primary positions: top, right, bottom, left, center.
```java
BorderPane layout = new BorderPane();
layout.setTop(new Label("top"));
```

**HBox**
- class enables UI components to be laid out in a *single horizontal row.*
```java
HBox layout = new HBox();
layout.setSpacing(10);
layout.getChildren().add(new Label("first"));
layout.getChildren().add(new Label("second"));
```

**VBox**
- class enables UI components to be laid out in a *single vertical column.*
```java
VBox layout = new VBox();
layout.setSpacing(10);
layout.getChildren().addAll(new Label("first"), new Label("second"));
```

**GridPane**
- can be used to lay the UI components in a grid.
```java
GridPane layout = new GridPane();

for (int x = 1; x <= 3; x++) {
	for (int y = 1; y <= 3; y++) {
		layout.add(new Button("" + x + ", " + y), x, y);
	}
}
```
> the following creates a 3x3 grid

#### Multiple Layouts on a Single Instance
- a typical setup involves using the BorderPane layout as the base with other layouts inside it.
```java
 BorderPane layout = new BorderPane();

HBox buttons = new HBox();
buttons.setSpacing(10);
buttons.getChildren().add(new Button("First"));
buttons.getChildren().add(new Button("Second"));

VBox texts = new VBox();
texts.setSpacing(10);
texts.getChildren().add(new Label("First"));
texts.getChildren().add(new Label("Second"));

TextArea textArea = new TextArea("");

layout.setTop(buttons);
layout.setLeft(texts);
layout.setCenter(textArea);
```

# Event handling
- functionality can be added to component events

- Button presses are handled using a class the implements the `EventHandler` interface.
- The type of the event in these cases is `ActionEvent`.
```java
TextField leftText = new TextField();
TextField rightText = new TextField();
Button button = new Button("Copy");
```

```java
button.setOnAction(new EventHandler<ActionEvent>() {
	@Override
	public void handle(ActionEvent event) {
		rightText.setText(leftText.getText());
	}
});
```

- implementation can be replaced by a Lambda expression
```java
button.setOnAction((event) -> {
	rightText.setText(leftText.getText());
});
```

- we often want an event handler to change the state of some object. to get a hold of an object, the event handler needs a reference to it.

- If we want to listen to changes made to a text field character by character, then we would use the interface `ChangeListener`
```java
leftText.textProperty().addListener(new ChangeListener<String>() {
	@Override
	public void changed(ObservableValue<? extends String> change,
			String oldValue, String newValue) {

		System.out.println(oldValue + " -> " + newValue);
		correctText.setText(newValue);
	}
})
```

```java
leftText.textProperty().addListener((change, oldValue, newValue) -> {
    System.out.println(oldValue + " -> " + newValue);
    rightText.setText(newValue);
});
```

**getting the longest string in an array of strings**
```java
String longest = Arrays.stream(parts)
	.sorted((s1, s2) -> s2.length() - s1.length())
	.findFirst()
	.get();
```

# Application's launch parameters
- we are able to launch JavaFx applications from outside of the `Application` class.
```java
package application;

import javafx.application.Application;

public class Main {

  public static void main(String[] args) {
      Application.launch(JavaFxApplication.class);
  }
}
```

- the application can also be provided run-time parameters as part of the `launch` method
- in addition, the `launch` method can be provided an unlimited number of strings, which are accessible with the `getParameters` method call.
	- the key-value pairs are given to the launch method in the form `--key = value`
```java
public static void main(String[] args) {
	Application.launch(JavaFxApplication.class,
		"--organization=Once upon a time",
		"--course=Title");
}
```

```java
@Override
public void start(Stage window) {
	Parameters params = getParameters();
	String organization = params.getNamed().get("organization");
	String course = params.getNamed().get("course");

	window.setTitle(organization + ": " + course);
	window.show();
}
```

# Multiple views
- views are created as Scene-objects and the transitioning between them

```java
Button back = new Button("Back ..");
Button forth = new Button(".. forth.");

Scene first = new Scene(back);
Scene second = new Scene(forth);

back.setOnAction((event) -> {
  window.setScene(second);
});

forth.setOnAction((event) -> {
  window.setScene(first);
});
```

==example== Showing Welcome Screen on Correct Password Input
```java
@Override
public void start(Stage window) throws Exception {

  // 1. Creating the view for asking a password

  // 1.1 Creating components to be used
  Label instructionText = new Label("Write the password and press Log in");
  PasswordField passwordField = new PasswordField();
  Button startButton = new Button("Log in");
  Label errorMessage = new Label("");

  // 1.2 creating layout and adding components to it
  GridPane layout = new GridPane();

  layout.add(instructionText, 0, 0);
  layout.add(passwordField, 0, 1);
  layout.add(startButton, 0, 2);
  layout.add(errorMessage, 0, 3);

  // 1.3 styling the layout
  layout.setPrefSize(300, 180);
  layout.setAlignment(Pos.CENTER);
  layout.setVgap(10);
  layout.setHgap(10);
  layout.setPadding(new Insets(20, 20, 20, 20));

  // 1.4 creating the view itself and setting it to use the layout
  Scene passwordView = new Scene(layout);

  // 2. Creating a view for showing the welcome message
  Label welcomeText = new Label("Welcome, this is the beginning!");

  StackPane welcomeLayout = new StackPane();
  welcomeLayout.setPrefSize(300, 180);
  welcomeLayout.getChildren().add(welcomeText);
  welcomeLayout.setAlignment(Pos.CENTER);

  Scene welcomeView = new Scene(welcomeLayout);

  // 3. Adding an event handler to the login button.
  // The view is changed if the password is right.
  startButton.setOnAction((event) -> {
	  if (!passwordField.getText().trim().equals("password")) {
		  errorMessage.setText("Unknown password!");
		  return;
	  }

	  window.setScene(welcomeView);
  });

  window.setScene(passwordView);
  window.show();
}
```

==example== Same Layout with Variable Content
```java
@Override
public void start(Stage window) throws Exception {

	// 1. Create main layout
	BorderPane layout = new BorderPane();

	// 1.1. Create menu for main layout
	HBox menu = new HBox();
	menu.setPadding(new Insets(20, 20, 20, 20));
	menu.setSpacing(10);

	// 1.2. Create buttons for menu
	Button first = new Button("First");
	Button second = new Button("Second");

	// 1.3. Add buttons to menu
	menu.getChildren().addAll(first, second);

	layout.setTop(menu);


	// 2. Add subviews and add them to the menu buttons
	// 2.1. Create subview layout
	StackPane firstLayout = createView("First view");
	StackPane secondLayout = createView("Second view");

	// 2.2. Add subviews to button. Pressing the buttons will change the view
	first.setOnAction((event) -> layout.setCenter(firstLayout));
	second.setOnAction((event) -> layout.setCenter(secondLayout));

	// 2.3. Set initial view
	layout.setCenter(firstLayout);

	// 3. Create main scene with layout 
	Scene scene = new Scene(layout);


	// 4. Show the main scene
	window.setScene(scene);
	window.show();
}

private StackPane createView(String text) {

	StackPane layout = new StackPane();
	layout.setPrefSize(300, 180);
	layout.getChildren().add(new Label(text));
	layout.setAlignment(Pos.CENTER);

	return layout;
}
```

==example== TicTacToe

```java
public class Game {
    private String[][] field;
    private int placed = 0;
    private String turn;
 
    public Game() {
        this.field = new String[][]{
	        {" ", " ", " "}, {" ", " ", " "}, {" ", " ", " "}
		};
        this.turn = "X";
    }
 
    public String getTurn() {
        return this.turn;
    }
 
    public String status(int x, int y) {
        if (indexOutOfBounds(x, y)) {
            return "";
        }
 
        return this.field[x][y];
    }
 
    public void place(int x, int y) {
        if (indexOutOfBounds(x, y)) {
            return;
        }
 
        if (!this.field[x][y].equals(" ")) {
            return;
        }
 
        this.field[x][y] = this.turn;
        this.placed++;
        this.changeTurn();
    }
 
    public void changeTurn() {
        if (this.turn.equals("X")) {
            this.turn = "O";
        } else {
            this.turn = "X";
        }
    }
 
    public boolean gameOver() {
        if (this.checkRows() || this.checkColumns() || this.checkDiagonals()) {
            return true;
        }
 
        if (this.placed == 9) {
            return true;
        }
 
        return false;
    }
 
    private boolean indexOutOfBounds(int x, int y) {
        if (x < 0 || y < 0 || x > 2 || y > 2) {
            return true;
        }
        return false;
    }
 
    private boolean checkRows() {
        for (int i = 0; i <= 2; i++) {
            if (this.field[i][0].equals(" ")) {
                return false;
            }
 
            if (this.field[i][0].equals(this.field[i][1]) 
            && this.field[i][1].equals(this.field[i][2])) {
                return true;
            }
        }
        return false;
    }
 
    private boolean checkColumns() {
        for (int i = 0; i < 3; i++) {
            if (this.field[0][i].equals(" ")) {
                return false;
            }
 
            if (this.field[0][i].equals(this.field[1][i]) 
            && this.field[1][i].equals(this.field[2][i])) {
                return true;
            }
        }
        return false;
    }
 
    private boolean checkDiagonals() {
        if (this.field[1][1].equals(" ")) {
            return false;
        }
 
        if (this.field[0][0].equals(this.field[1][1]) 
        && this.field[1][1].equals(this.field[2][2])) {
            return true;
        }
 
        if (this.field[0][2].equals(this.field[1][1]) 
        && this.field[1][1].equals(this.field[2][0])) {
            return true;
        }
 
        return false;
    }
}
```

```java
public class TicTacToeApplication extends Application {
 
    @Override
    public void start(Stage window) throws Exception {
        Game game = new Game();
 
        Label info = new Label("Turn: X");
 
        BorderPane layout = new BorderPane();
        layout.setTop(info);
 
        GridPane field = new GridPane();
        field.setPrefSize(300, 180);
        field.setAlignment(Pos.CENTER);
        field.setVgap(10);
        field.setHgap(10);
        field.setPadding(new Insets(10));
 
        for (int x = 0; x < 3; x++) {
            for (int y = 0; y < 3; y++) {
                Button btn = new Button(game.status(x, y));
                btn.setFont(Font.font("Monospaced", 40));
 
                field.add(btn, x, y);
 
                int rx = x;
                int ry = y;
                btn.setOnMouseClicked((event) -> {
                    if (game.gameOver()) {
                        return;
                    }
 
                    game.place(rx, ry);
                    btn.setText(game.status(rx, ry));
                    info.setText("Turn: " + game.getTurn());
 
                    if (game.gameOver()) {
                        info.setText("The end!");
                    }
 
                });
            }
        }
        layout.setCenter(field);
 
        Scene scene = new Scene(layout);
        window.setScene(scene);
        window.show();
    }
 
    public static void main(String[] args) {
        launch(TicTacToeApplication.class);
    }
 
}
```

---

# Data visualization

### Charts
#### Line Chart
```java
@Override
    public void start(Stage stage) {
        NumberAxis xAxis = new NumberAxis(2013, 2018, 1);
        NumberAxis yAxis = new NumberAxis(0, 100, 10);
        
        xAxis.setLabel("Year");
        yAxis.setLabel("Ranking");
        
        LineChart<Number, Number> lineChart = new LineChart<>(xAxis, yAxis);
        lineChart.setTitle("University of Helsinki, Shanghai ranking");
        
        XYChart.Series data = new XYChart.Series();
        data.setName("University of Helsinki");
        
        data.getData().add(new XYChart.Data(2013, 76));
        data.getData().add(new XYChart.Data(2014, 73));
        data.getData().add(new XYChart.Data(2015, 67));
        data.getData().add(new XYChart.Data(2016, 56));
        data.getData().add(new XYChart.Data(2017, 56));
        
        lineChart.getData().add(data);
        
        Scene scene = new Scene(lineChart, 640, 480);
        stage.setScene(scene);
        stage.show();
    }
```

**ideally, we would read the data into a suitable data structure, after which we can go through the structure and add the data contained in it to the chart.**
```java
 public static Map<String, Map<Integer, Double>> getData(String file) {
        Map<String, Map<Integer, Double>> values = new HashMap<>();
 
        try {
            List<String> lines = Files.readAllLines(Paths.get(file));
 
            String[] years = lines.get(0).split("\t");
 
            for (int i = 1; i < lines.size(); i++) {
                String[] parts = lines.get(i).split("\t");
                String party = parts[0];
                Map<Integer, Double> partyData = new HashMap<>();
 
                for (int j = 1; j < parts.length; j++) {
                    int year = Integer.parseInt(years[j]);
                    if (!parts[j].equals("-")) {
                        double relativeSupport = Double.parseDouble(parts[j]);
                        partyData.put(year, relativeSupport);
                    }
                }
 
                values.put(party, partyData);
            }
 
        } catch (IOException e) {
            System.out.println("Error: " + e.getMessage());
        }
 
        return values;
    }
```

```java
Map<String, Map<Integer, Double>> values = geData(/*FILE_NAME*/);

// go through the parties and add them to the chart
values.keySet().stream().forEach(party -> {
    // a different data set for every party
    XYChart.Series data = new XYChart.Series();
    data.setName(party);

    // add the party's support numbers to the data set
    values.get(party).entrySet().stream().forEach(pair -> {
        data.getData().add(new XYChart.Data(pair.getKey(), pair.getValue()));
    });

    // and add the data set to the chart
    lineChart.getData().add(data);
});
```

