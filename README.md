# JavaSE_Calculator

This is the Java application whole code (file calculator.java):

```java
public class Calculator {
    public static void main(String[] args) {
        if (args.length != 3) {
            System.out.println("Invalid number of arguments. Usage: java Calculator <operand1> <operator> <operand2>");
            return;
        }

        double operand1, operand2;
        try {
            operand1 = Double.parseDouble(args[0]);
            operand2 = Double.parseDouble(args[2]);
        } catch (NumberFormatException e) {
            System.out.println("Invalid operands. Operands must be numbers.");
            return;
        }

        char operator = args[1].charAt(0);
        double result;
        switch (operator) {
            case '+':
                result = operand1 + operand2;
                System.out.println("Result: " + result);
                break;
            case '-':
                result = operand1 - operand2;
                System.out.println("Result: " + result);
                break;
            case 'x':
                result = operand1 * operand2;
                System.out.println("Result: " + result);
                break;
            case '/':
                if (operand2 == 0) {
                    System.out.println("Cannot divide by zero.");
                    return;
                }
                result = operand1 / operand2;
                System.out.println("Result: " + result);
                break;
            default:
                System.out.println("Invalid operator. Supported operators: +, -, x, /");
        }
    }
}
```

To run this application we first compile the Java file for creating the class file:

```
javac Calculator.java
```

The output would be: calculator.class

Now we can run the application sending 

```
java Calculator <operand1> <operator> <operand2>
```

For example: 
```
java Calculator 5 + 3
```
