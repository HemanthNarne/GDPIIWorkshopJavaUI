1. Create a Java Project and name it as a CalculatorJavaFX as shown below
![CreateProject](Capture.JPG)
![NameProject](Capture1.JPG)
2. Extended the calculator class to the Application class
#####      /*
#####      * To change this license header, choose License Headers in Project Properties.
#####      * To change this template file, choose Tools | Templates
#####      * and open the template in the editor.
#####      */
#####      package calculator;

#####     import java.util.ArrayList;
#####     import java.util.Arrays;
#####     import java.util.List;
#####     import javafx.application.Application;
#####     import static javafx.application.Application.launch;
#####     import javafx.beans.binding.Bindings;
#####     import javafx.beans.property.DoubleProperty;
#####     import javafx.beans.property.SimpleDoubleProperty;
#####     import javafx.event.ActionEvent;
#####     import javafx.geometry.Insets;
#####     import javafx.geometry.Pos;
#####     import javafx.scene.Scene;
#####     import javafx.scene.control.Button;
#####     import javafx.scene.control.Label;
#####     import javafx.scene.layout.GridPane;
#####     import javafx.stage.Stage;

#####     /**
#####      *
#####      * @author Santhosh Bonala
#####      */
#####      public class CalculatorJavaFX extends Application {

#####         // Declare a variable of type Label to display result
#####         private Label reslabel;
#####         // Declare and initialize a variable of type string inorder to store the operator #####          selected
#####          private ArrayList<String> operator = new ArrayList<>();
#####          // Declare and initialize a variable of type Integer inorder to store the operand 1
#####          private double num1 = 0;
#####          // Declare and initialize a variable of type DoubleProperty for binding the fontsize
#####          private DoubleProperty fontSize = new SimpleDoubleProperty(10);
#####          // Declare and initialize an arraylist for all the button labels
#####          private List<String> btnlabels = Arrays.asList("0", "1", "2", "3", "4", "5", "6",                "7", "8", "9", "C", "=", "+", "-", "*", "/", ".");

#####          private Button btnclear;
#####          // declare and initialize an arraylist of buttons
#####          private ArrayList<Button> btns = new ArrayList<>();

#####          private ArrayList<Double> operands = new ArrayList<>();

#####          private Boolean cancelflag = true, divideoverflowflag = true, operatorflag = false;

#####          // This method is called from launch static method of Application class by the JVM itself
#####          @Override
#####          public void start(Stage primaryStage) {
#####              //create a grid layout
#####              GridPane gridPane = new GridPane();
#####              // set vertical space between grid cells to be 5px
#####              gridPane.setVgap(5);
#####              // set Horizontal space between grid cells to be 5px
#####              gridPane.setHgap(5);
#####              //Setting the padding  
#####              gridPane.setPadding(new Insets(10));
#####              //Setting the Grid alignment 
#####              gridPane.setAlignment(Pos.CENTER);

#####              // create instances of each button and add them to arraylist
#####              for (String str : btnlabels) {
#####                Button btn = new Button(str);
#####                  btn.setMinWidth(50);
#####                  btn.prefWidthProperty().bind(gridPane.widthProperty());
#####               btn.prefHeightProperty().bind(gridPane.heightProperty());
#####               btn.setOnAction(e -> btncode(e));
#####               btns.add(btn);
#####                }

#####               // create a result label and set width and color
#####               reslabel = new Label("");
#####               reslabel.setStyle("-fx-alignment: RIGHT;-fx-border-color: black;-fx-font-size:                            2em;-fx-background-color: white;");
#####               reslabel.setMinWidth(220);
#####               reslabel.setMinHeight(20);
#####               reslabel.prefWidthProperty().bind(gridPane.widthProperty());
#####               reslabel.prefHeightProperty().bind(gridPane.heightProperty());

#####               // create a clear button and set width and font styles
#####               btnclear = new Button("Clear");
#####               btnclear.setMinWidth(160);
#####               btnclear.setStyle("-fx-background-color: #f9613b;                                                 -fx-text-fill: white; -fx-font-weight: bold;");
#####               btnclear.prefWidthProperty().bind(gridPane.widthProperty());
#####               btnclear.prefHeightProperty().bind(gridPane.heightProperty());
#####               btnclear.setOnAction(e -> btncode(e));

#####               // attach the created nodes to the layout
#####               gridPane.add(reslabel, 0, 0, 4, 1);
#####               gridPane.add(btnclear, 0, 1, 3, 1);
#####               gridPane.add(btns.get(16), 3, 1);
#####               gridPane.addRow(2, btns.get(7), btns.get(8), btns.get(9), btns.get(15));
#####               gridPane.addRow(3, btns.get(4), btns.get(5), btns.get(6), btns.get(14));
#####               gridPane.addRow(4, btns.get(1), btns.get(2), btns.get(3), btns.get(13));
#####               gridPane.addRow(5, btns.get(10), btns.get(0), btns.get(11), btns.get(12));

#####               // create a scene with gridpane as scene graph
#####               Scene scene = new Scene(gridPane, 400, 350);

#####               // Bind the font size with the size of the window
#####               fontSize.bind(scene.widthProperty().add(scene.heightProperty()).divide(50));
#####               gridPane.styleProperty().bind(Bindings.concat("-fx-font-size: ",                                      fontSize.asString(), ";",
#####                     "-fx-base: rgb(156,179,216);"));
#####               // set title for the stage (window)
#####               primaryStage.setTitle("Calculator");

#####               //set scene for the stage
#####               primaryStage.setScene(scene);

#####               //show the stage
#####               primaryStage.show();
#####            }

#####             // For every button click the below method is called to process the request
#####                 public void btncode(ActionEvent e) {
#####           // if clear button is clicked remove the text from result label
#####            if ("Clear".equals(((Button) e.getSource()).getText())) {
#####                reslabel.setText("");
#####                operator.clear();
#####                operands.clear();
#####                divideoverflowflag = true;
#####                cancelflag = true;
#####                operatorflag = false;
#####                return;
#####            }
#####            // if cancel is clicked the last element from result label is removed
#####            if ("C".equals(((Button) e.getSource()).getText())) {
#####                if (!reslabel.getText().equals("") && cancelflag && divideoverflowflag) {
#####           reslabel.setText(reslabel.getText().substring(0, reslabel.getText().length() - 1));
#####                }
#####                return;
#####            }
#####            // if any of the operator is clicked, store the operator and empty the result label
#####            if (divideoverflowflag && ("+".equals(((Button) e.getSource()).getText())
#####              || "-".equals(((Button) e.getSource()).getText())
#####              || "*".equals(((Button) e.getSource()).getText())
#####              || "/".equals(((Button) e.getSource()).getText()))) {
#####                   if (operatorflag) {
#####                       cancelflag = true;
#####                       if (!reslabel.getText().equals("")) {
#####                           operands.add(Double.parseDouble(reslabel.getText()));
#####                           operator.add(((Button) e.getSource()).getText());
#####                           reslabel.setText("");
#####                           operatorflag = false;
#####                       }
#####                   }
#####              return;
#####          }

#####              // if equals operator is clicked then the required operation is performed and                      result is loaded will be loaded into result label
#####              if ("=".equals(((Button) e.getSource()).getText()) && divideoverflowflag) {
#####                  if (operatorflag) {
#####                      int i = 0;
#####                      double result = operands.get(i);
#####                      operands.add(Double.parseDouble(reslabel.getText()));
#####                      for (String op : operator) {
#####               i = i + 1;
#####               if (!op.equals("")) {
#####                   switch (op) {
#####                       case "+":
#####                result += operands.get(i);
#####                break;
#####                       case "-":
#####                result -= operands.get(i);
#####                break;
#####                       case "*":
#####                result *= operands.get(i);
#####                break;
#####                       case "/":
#####                if (operands.get(i) == 0) {
#####                    reslabel.setText("Cannot divide by zero");
#####                    cancelflag = false;
#####                    divideoverflowflag = false;
#####                    return;
#####                }
#####                result /= operands.get(i);
#####                break;
#####                   }
#####               }

#####              }
#####              operands.clear();
#####              operator.clear();
#####              cancelflag = false;
#####              if ((result == Math.floor(result)) && !Double.isInfinite(result)) {
#####               reslabel.setText(String.format("%,d", (int) result));
#####               return;
#####              }
#####              reslabel.setText(String.format("%.3f", result));
#####              return;
#####          } else {
#####              return;
#####          }
#####      }

#####       if (divideoverflowflag) {
#####       cancelflag = true;
#####       operatorflag = true;
#####       // if any of the number is clicked, the respective number will be added to the result               label
#####         reslabel.setText(reslabel.getText() + ((Button) e.getSource()).getText());
#####       }

#####        }

#####        /**
#####        * @param args the command line arguments
#####        */
#####        public static void main(String[] args) {
#####           launch(args);
#####        }

#####    }
