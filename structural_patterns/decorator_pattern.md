# Decorator Pattern
## Overview
Decorator pattern is a **structural patttern** that lets you **attach new behaviours** 
to objects by placing these objects **inside special wrapper objects** that contain the behaviours.
**Multiple decorators** can be added to a single object and the behaviours **stacked recursively**.

## Advantages
- Allows for **extension** of an object's behaviour without making a new subclass.
- Can add or remove responsibilities from an object at **runtime**.
- Can **combine several behaviours** by wrapping an object into multiple decorators.
- Satisfies **single responsibility principle** by dividing a monolithic class into several smaller decorating classes.
- Satisfies **open/closed principle** by not requiring changes in the original class when adding new functionality.

## Disadvantages
- Hard to **remove specific wrappers** from the wrappers stack.
- **Decorating order** often matters, introducing complexity with ordering.

## Applicability
- When you need to be able to **assign extra behaviours to objects** at **runtime** without breaking the code that uses these objects.
- When it's awkward or not possible to extend an object's behaviour using **inheritance**.
- When you have **multiple optional behaviours** that you want to be able to dynamically add to objects.

## Example
```java
public interface Coffee {
    public double getCost();
    public String getIngredients();
}

public class SimpleCoffee implements Coffee {
    @Override
    public double getCost() {
        return 1;
    }

    @Override
    public String getIngredients() {
        return "Coffee";
    }
}

public abstract class CoffeeDecorator implements Coffee {
    private final Coffee decoratedCoffee;

    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }

    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }

    @Override
    public String getIngredients() {
        return decoratedCoffee.getIngredients();
    }
}

public class WithMilk extends CoffeeDecorator {
    public WithMilk(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        return super.getCost() + 0.5;
    }

    @Override
    public String getIngredients() {
        return super.getIngredients() + ", Milk";
    }
}

public class WithSprinkles extends CoffeeDecorator {
    public WithSprinkles(Coffee coffee) {
        super(coffee);
    }

    @Override
    public double getCost() {
        return super.getCost() + 0.2;
    }

    @Override
    public String getIngredients() {
        return super.getIngredients() + ", Sprinkles";
    }
}

public class Main {
    public static void printInfo(Coffee coffee) {
        System.out.println("Cost: " + coffee.getCost())
                           + "; Ingredients: " + coffee.getIngredients());
    }

    public static void main(String[] args) {
        Coffee coffee = new SimpleCoffee();
        printInfo(coffee);

        // Decorate coffee once.
        coffee = new WithMilk(coffee);
        printInfo(coffee);

        // Decorate coffee again.
        // Tracing path of cost calls:
            // coffee.getCost()
            // -> WithSprinkles.getCost()
            // -> CoffeeDecorator.getCost()
            // -> WithMilk.getCost()
            // -> CoffeeDecorator.getCost()
            // -> SimpleCoffee.getCost()
        // Essentially: 
        // Alternate returns to the base CoffeeDecorator
        // each time but with one less decorating layer.
        coffee = new WithSprinkles(coffee);
        printInfo(coffee);

        // Output:
        // Cost: 1.0; Ingredients: Coffee
        // Cost: 1.5; Ingredients: Coffee, Milk
        // Cost: 1.7; Ingredients: Coffee, Milk, Sprinkles
    }
}
```
