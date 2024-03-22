- [[#Graphical user interfaces]]
	- [[#UI components and their layout]]

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

