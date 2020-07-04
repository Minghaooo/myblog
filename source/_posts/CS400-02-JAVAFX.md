---
title: CS400-02-JAVAFX
date: 2020-07-02 22:18:54
tags: JAVA
---
### what is an event?
 普通编程是串行的，但是在GUI中我们不知道下一次要做什么，需要根据用户的行为来决定。
 Events are asynchronous, can happen any time.
 <!--more-->
Event is triggered when the user does something with the GUI.
The code is called "event holder" holder.
```
public interface EventHander<T extends Event> extends Eventlistener{
    public void handle(T event);

}

```

* A Scene is a JavaFX component that contains all graphical components that are displayed together and is available via the import statement import javafx.scene.Scene;
*  A Pane is a JavaFX component that controls the layout, i.e., position and size, of graphical components and is available via the import statement import javafx.scene.layout.Pane;. The statement pane = new Pane(); creates an empty Pane object. The statement  scene = new Scene(pane); creates a new Scene containing the pane object.

Create and add graphical components to a pane: 
* A TextField is a JavaFX GUI component that enables a programmer to display a line of text and is available via the import statement import javafx.scene.control.TextField;. 
* The statement outputField = new TextField(); creates a TextField object. 
* A TextField's setText() method specifies the text that will be displayed. Ex: outputField.setText("An hourly ... ");. 
* a TextField allows users to modify the text for the purposes of input (discussed elsewhere).
*  A program can use TextField's setEditable() method with an argument of false to prevent users from editing the text, as in outputField.setEditable(false);. 
*  A TextField's width can be set using the setPrefColumnCount() method. Ex: outputField.setPrefColumnCount(22) sets the width to 22 columns.


Set and display scene: 
* Stage's setScene() method sets the scene that will be displayed, as in applicationStage.setScene(scene);. 
* The setTitle() method specifies the text that will be displayed as the application's title. Ex: applicationStage.setTitle("Salary"); displays "Salary" in the application's title bar. 
* Stage's show() method makes the stage visible, which displays the application's window to the user. Ex: applicationStage.show(); 
* displays the application's window with the title "Salary" and text "An hourly wage of $20/hr yields $40000/yr.