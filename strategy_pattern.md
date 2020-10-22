# Strategy Pattern
## Overview
Strategy pattern is a **behavioural pattern** in which objects are created which represent **various strategies** and are **held in a context object**. This context **delegates** to the strategy object and can hence have **differing behaviour** depending on the specific strategy object.

## Advantages
- **Encapsulates** a family of algorithms by **extracting what changes** to a different class.
- Enables behaviour to change during **run-time**.
- **Decouples** between the behaviour and the context class that uses the behaviour.

## Disadvantages
- Increases number of objects
- Client must be aware of different strategies.

## Applicability
- When you want different variants of an algorithm within an object and be able to **change them during runtime**.
- When you have a lot of **similar classes** that only **differ** in the way they **execute some behaviour**.
- When you want to **isolate business logic** of a class **from implementation details of algorithms** that may not be as important in the context of the logic.

## Example
```java
public abstract class Duck {
    FlyBehaviour flyBehaviour;
    QuackBehaviour quackBehaviour;

    public void performFly() {
        flyBehaviour.fly();
    }

    public void performQuack() {
        quackBehaviour.quack();
    }

    public setFlyBehaviour(FlyBehaviour f) {
        flyBehaviour = f;
    } 

    public setQuackBehaviour(QuackBehaviour q) {
        quackBehaviour = q;
    }
}

public class MallardDuck {
    public MallardDuck() {
        flyBehaviour = new FlyWithWings();
        quackBehaviour = new Quack();
    }
} 

public interface FlyBehaviour {
    public abstract void fly();
}

public class FlyWithWings {
    public void fly() {
        // implement duck flying
    }
}

public class FlyNoWay {
    public void fly() {
        // do nothing
    }
}

public interface QuackBehaviour {
    public abstract void quack();
}

public class Quack {
    public void quack() {
        // implement duck quack
    }
}

public class Squeak {
    public void quack() {
        // implement rubber duck squeak
    }
}
```

Each duck **has a** fly behaviour and a quack behaviour. Instead of inheritance, the ducks get their behaviour by being **composed** with the right behaviour objects and **delegate** to their behaviour objects.