- [[#Graphical user interfaces]]
	- [[#UI components and their layout]]
- [[#Event handling]]

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