---
tags:
  - Java
---
# Introduction to JavaFX
JavaFX is a software platform for creating desktop applications and Rich Internet Applications (RIAs) that can run across multiple devices. It's the successor to Java Swing and provides a more modern, feature-rich framework for building user interfaces.

# Key Components

## Stage and Scene
- **Stage**: The top-level container (window) in JavaFX
- **Scene**: The container that holds all content in a window
```java
Stage primaryStage = new Stage();
Scene scene = new Scene(root, 800, 600);
primaryStage.setScene(scene);
primaryStage.show();
```

## Layout Containers
1. **VBox**: Arranges nodes vertically
```java
VBox vbox = new VBox(10); // 10 is spacing between elements
vbox.getChildren().addAll(button1, button2, label);
```

2. **HBox**: Arranges nodes horizontally
```java
HBox hbox = new HBox(10);
hbox.getChildren().addAll(label, textField);
```

3. **GridPane**: Arranges nodes in a flexible grid
```java
GridPane grid = new GridPane();
grid.add(label, 0, 0);    // column 0, row 0
grid.add(textField, 1, 0); // column 1, row 0
```
4. BorderPane: Divides space into top, bottom, left, right, and center regions
```java
BorderPane borderPane = new BorderPane();
borderPane.setTop(menuBar);
borderPane.setCenter(content);
borderPane.setBottom(statusBar);
```

## UI Controls

### Basic Controls
1. **Button**
```java
Button button = new Button("Click Me");
button.setOnAction(e -> System.out.println("Button clicked!"));
```

2. **Label**
```java
Label label = new Label("Hello JavaFX");
label.setFont(Font.font("Arial", FontWeight.BOLD, 14));
```

3. **TextField**
```java
TextField textField = new TextField();
textField.setPromptText("Enter text here");
```

4. **TextArea**
```java
TextArea textArea = new TextArea();
textArea.setPrefRowCount(5);
```

## Event Handling
JavaFX uses event handlers and listeners for handling user interactions:

```java
button.setOnAction(new EventHandler<ActionEvent>() {
    @Override
    public void handle(ActionEvent event) {
        // Handle button click
    }
});

// Or using lambda expression
button.setOnAction(e -> {
    // Handle button click
});
```

## CSS Styling
JavaFX supports CSS styling for customizing the appearance:

```java
// In Java
utton.setStyle("-fx-background-color: #0000FF; -fx-text-fill: white;");

// Or in CSS file
.button {
   -fx-background-color: #0000FF;
    -fx-text-fill: white;
}
```

## Animation
JavaFX provides various animation capabilities:

```java
// Fade transition example
FadeTransition fade = new FadeTransition(Duration.seconds(2), node);
fade.setFromValue(1.0);
fade.setToValue(0.0);
fade.play();
```

## FXML
FXML is an XML-based language that provides a convenient way to create user interfaces:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<?import javafx.scene.control.Button?>
<?import javafx.scene.layout.VBox?>

<VBox xmlns:fx="http://javafx.com/fxml" 
      fx:controller="com.example.Controller">
    <Button text="Click Me" 
           onAction="#handleButtonClick"/>
</VBox>
```

## Best Practices
1. Use FXML to separate UI design from logic
2. Follow MVC (Model-View-Controller) pattern
3. Use CSS for styling instead of inline styles
4. Handle exceptions appropriately in event handlers
5. Use Platform.runLater() for updating UI from non-JavaFX threads

## Common Patterns

### Initializing JavaFX Application
```java
public class MyApp extends Application {
    @Override
    public void start(Stage primaryStage) {
        Parent root = FXMLLoader.load(getClass().getResource("view.fxml"));
        Scene scene = new Scene(root);
        primaryStage.setScene(scene);
        primaryStage.show();
    }

    public static void main(String[] args) {
        launch(args);
    }
}
```

### Controller Class
```java
public class Controller implements Initializable {
    @FXML
    private Button myButton;
    
    @Override
    public void initialize(URL url, ResourceBundle rb) {
        // Initialize controller
    }
    
    @FXML
    private void handleButtonClick(ActionEvent event) {
        // Handle button click
    }
}
```