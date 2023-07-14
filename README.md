# JavaSE_Calculator

## Java application source code

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

## How to run this application

we first compile the Java file for creating the class file:

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

## How to setup the Github actions

It is very important to structure the project folders following this schema:

```
JavaSE_Calculator/
├── src/
│   └── (source code files)
└── pom.xml
```

Then we set the pom.xml file:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.example</groupId>
    <artifactId>calculator</artifactId>
    <version>1.0.0</version>

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- Add any dependencies your application requires here -->
    </dependencies>
</project>
```

We also have to create a build.yml file. Inside the yml file we also create the Javadocs:

```yml
name: Java CI

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:

    runs-on: ubuntu-latest

    steps:
    - uses: actions/checkout@v2

    - name: Set up JDK 11
      uses: actions/setup-java@v2
      with:
        java-version: 11
        distribution: 'adopt'
        server-id: github
        server-username: GITHUB_ACTOR
        server-password: GITHUB_TOKEN

    - name: Use Node.js 16.x
      uses: actions/setup-node@v2
      with:
        node-version: 16

    - name: Build with Maven
      run: mvn package

    - name: Generate JavaDocs
      run: mvn javadoc:javadoc

    - name: Run tests
      run: mvn test
```

This is the yml file ouput:

![github actions output](https://github.com/luiscoco/JavaSE_Calculator/assets/32194879/9dd1c976-7ccc-48f8-966c-34c60c9d80a1)

## How to include Unit Testing in your Java application and Github actions workflow.

To include JUnit testing in your Java application and in your GitHub Actions workflow, you will need to make the following modifications:

Update your pom.xml file to include the JUnit dependency. Open the pom.xml file and add the following <dependency> section within the <dependencies> section:

```xml

<dependencies>
    <!-- Other dependencies -->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

This adds the JUnit dependency to your project and sets its scope to test, indicating that it is only required for testing purposes.

Write JUnit tests for your application. Create a new Java class (e.g., CalculatorTest) in the same package as your application, and write test methods using JUnit annotations. Here's an example of a JUnit test class:

```java
import org.junit.Test;
import static org.junit.Assert.*;

public class CalculatorTest {
    @Test
    public void testAddition() {
        Calculator calculator = new Calculator();
        double result = calculator.add(2, 3);
        assertEquals(5, result, 0);
    }

    @Test
    public void testDivision() {
        Calculator calculator = new Calculator();
        double result = calculator.divide(10, 2);
        assertEquals(5, result, 0);
    }
}
```

Make sure to import the necessary JUnit classes (Test and Assert) and use assertions to verify the expected results.

Update your GitHub Actions workflow to run the JUnit tests. Open the build.yml file and add the following step after the Maven build step:

```yaml
- name: Run JUnit tests
  run: mvn test
```

This step executes the mvn test command, which runs the JUnit tests defined in your project.

Save the pom.xml and build.yml files, commit them to your GitHub repository, and re-run the GitHub Actions workflow. The Maven build process will now execute the JUnit tests as part of the workflow run.

With these modifications, your application will include JUnit testing, and the GitHub Actions workflow will run the tests to ensure the correctness of your code.
