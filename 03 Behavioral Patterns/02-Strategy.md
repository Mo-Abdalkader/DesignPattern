# Strategy Pattern

## Problem
The Strategy Pattern addresses the need to define a family of algorithms or behaviors, encapsulate each one, and make them interchangeable. This pattern is useful when you have multiple variations of a behavior within a class hierarchy, and you want to avoid using inheritance to implement each variation. It helps in scenarios where you need to switch behaviors dynamically at runtime without altering the class that uses these behaviors.

## Solution
The Strategy Pattern suggests the following approach:

**1- Define Behavior Interfaces:** 
- Create interfaces for each behavior that can vary. These interfaces declare methods that encapsulate the behavior.

**2- Implement Concrete Behaviors:**
- Provide different implementations of these behavior interfaces. Each implementation represents a specific variant of the behavior.

**3- Use Composition:**
- Instead of embedding behavior directly into a class (using inheritance), use composition to include behavior objects as components of the class. This allows behaviors to be selected or changed at runtime.

## Examples 
### 1- Duck Lack Problem
Imagine you are building a duck simulator. You need to create different types of ducks, such as Mallard Ducks and Rubber Ducks, each with its own unique behaviors. Initially, you might think of using inheritance to model these ducks:

```java
public class Duck {
    public void quack() {
        System.out.println("Quack!");
    }

    public void fly() {
        System.out.println("I'm flying!");
    }
}

public class MallardDuck extends Duck {
    // Inherits quack() and fly() from Duck
}

public class RubberDuck extends Duck {
    @Override
    public void quack() {
        System.out.println("Squeak!");
    }

    @Override
    public void fly() {
        // Rubber ducks can't fly
    }
}
```

#### Problems with Inheritance-Based Design

**Code Duplication:**
- As new duck types are added, you might need to override behaviors in many subclasses, leading to duplicate code if multiple subclasses share the same behavior modification.

**Rigid Design:**
- If you want to change a behavior (e.g., how ducks fly or quack), you need to modify the superclass or all subclasses, leading to potential errors and inconsistencies.

**Lack of Flexibility:**
- Subclasses are tightly coupled to the superclass. Changes in the superclass can unintentionally affect all subclasses.
- Cannot change behaviors at runtime. Each duck has a fixed set of behaviors determined at compile time.

**Maintenance Overhead:**
- With an increasing number of subclasses, the design becomes harder to manage, understand, and maintain.

**Violation of the Single Responsibility Principle:**
- The Duck class handles both the behaviors (quacking and flying) and the characteristics of ducks, making it responsible for too many things.

### Solution: Applying the Strategy Pattern
The Strategy Pattern helps solve these issues by promoting composition over inheritance. It allows you to define a family of algorithms or behaviors, encapsulate each one, and make them interchangeable. This way, you can change the behavior of a duck at runtime without modifying the duck classes.

#### 1- Define Behavior Interfaces:
```java
// QuackBehavior interface
public interface QuackBehavior {
    void quack();
}

// FlyBehavior interface
public interface FlyBehavior {
    void fly();
}
```

#### 2- Implement Concrete Behaviors:
```java
// Concrete implementation of QuackBehavior interface
public class Quack implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("Quack!");
    }
}

// Another implementation of QuackBehavior interface
public class Squeak implements QuackBehavior {
    @Override
    public void quack() {
        System.out.println("Squeak!");
    }
}

// Concrete implementation of FlyBehavior interface
public class FlyWithWings implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("I'm flying with wings!");
    }
}

// Another implementation of FlyBehavior interface
public class FlyNoWay implements FlyBehavior {
    @Override
    public void fly() {
        System.out.println("I can't fly!");
    }
}
```

#### 3- Use Composition in Duck Class:
```java
// Duck class with Strategy Pattern
public abstract class Duck {
    // Declare behavior interfaces as instance variables
    protected QuackBehavior quackBehavior;
    protected FlyBehavior flyBehavior;

    // Setter methods to set behaviors dynamically
    public void setQuackBehavior(QuackBehavior quackBehavior) {
        this.quackBehavior = quackBehavior;
    }

    public void setFlyBehavior(FlyBehavior flyBehavior) {
        this.flyBehavior = flyBehavior;
    }

    // Abstract method for displaying the duck
    public abstract void display();

    // Common methods that use behaviors
    public void performQuack() {
        quackBehavior.quack();
    }

    public void performFly() {
        flyBehavior.fly();
    }

    // Other duck methods...
}
```

#### 4- Create Specific Duck Types:
```java
// MallardDuck extends Duck
public class MallardDuck extends Duck {
    // Constructor initializes with specific behaviors
    public MallardDuck() {
        quackBehavior = new Quack();
        flyBehavior = new FlyWithWings();
    }

    @Override
    public void display() {
        System.out.println("I'm a Mallard duck");
    }
}

// RubberDuck extends Duck
public class RubberDuck extends Duck {
    // Constructor initializes with specific behaviors
    public RubberDuck() {
        quackBehavior = new Squeak();
        flyBehavior = new FlyNoWay();
    }

    @Override
    public void display() {
        System.out.println("I'm a Rubber duck");
    }
}
```

#### 5- Main Program to Test Ducks:
```java
public class MiniDuckSimulator {
    public static void main(String[] args) {
        Duck mallard = new MallardDuck();
        mallard.display();
        mallard.performQuack();
        mallard.performFly();

        Duck rubberDuckie = new RubberDuck();
        rubberDuckie.display();
        rubberDuckie.performQuack();
        rubberDuckie.performFly();

        // Dynamically change behaviors at runtime
        rubberDuckie.setQuackBehavior(new Quack());
        rubberDuckie.setFlyBehavior(new FlyWithWings());
        rubberDuckie.performQuack();
        rubberDuckie.performFly();
    }
}
```

### Explanation
**Design Pattern:** 
- Strategy Pattern allows ducks to have different behaviors (quacking and flying) that can be interchanged dynamically.
**Problem:**
- Without the Strategy Pattern, implementing different behaviors for ducks would require multiple subclasses or conditional statements, leading to code duplication and rigidity.
**Solution:**
- By defining behaviors as separate interfaces and implementing them as concrete classes, and then using composition in the Duck class, behaviors can be easily swapped at runtime without affecting the Duck subclasses or clients using the ducks.


In summary, the Strategy Pattern enhances flexibility, promotes code reuse, and simplifies maintenance by encapsulating behaviors and enabling dynamic changes. This makes it ideal for scenarios where you anticipate varying behaviors within a class hierarchy, such as different types of ducks with different ways of quacking and flying.
