# Visitor Pattern
## Overview
Visitor is a **behavioural pattern** that lets you **separate algorithms from the objects** on which they operate. It allows **new behaviours** to be added to **existing classes** without modifying them (mostly).

## Advantages
- Satisfies **open/closed principle** as **new behaviours can be added** that work with **existing objects** of different classes **without changing these classes**.
- Satisfies **single responsibility principle** as multiple versions of the same beavhiour can be moved into the same class.

## Disadvantages
- ALl **visitors must be updated** when a **new class gets added** to the element heirarchy.
- Visitors might **lack necessary access** to **private fields and methods** of the elements that they're supposed to work with.

## Applicability
- When you need to perform an operation on all elements of a **complex object structure**.
- **Clean up business logic** of auxiliary behaviours. Primary classes are made more focused on their main jobs as **other behaviours are extracted** into a set of visitor classes.
- When a behaviour makes sense **only in some classes** of a class hierarchy, but not in others. Only the necessary visiting methods need to be implemented, the rest can be left empty.

## Static vs Dynamic Binding
- **Static binding** refers to execution where types of objects are determined/known at **compile time**.
- **Dynamic binding** refers to execution where types of objects are determined/known at **runtime**.
- **Overloaded methods** are binded using **static binding**.
- **Overridden methods** are binded using **dynamic binding**.
- Static binding prevents us from using overloaded methods with single dispatch in our Visitor class. 

```java
class Animal { }
class Dog extends Animal { }

public class Test {
    void callEat(Animal animal) {
        System.out.println("Animal is eating");
    }

    void callEat(Dog dog) {
        System.out.println("Dog is eating");
    }

    public static void main(String args[]) {
        Test t = new Test();

        // While a is also an instance of Dog,
        // at compile time the compiler only knows it is Animal,
        // so callEat(Animal animal) is used instead.
        Animal a = new Dog();
        t.callEat(a); // prints "Animal is eating"

        // We give the compiler enough information
        // to determine b is Dog at compile time
        // and so callEat(Dog dog) is used instead.
        Dog b = new Dog();
        t.callEat(b); // prints "Animal is eating"
    }
}
```

## Double Dispatch
- Double dispatch is a trick that allows **dyanamic binding** to work with **overloaded methods**.
- In the visitor pattern: 
    - 1. Object accepts visitor.
    - 2. Object calls visitor method with itself (`this`) as argument.
    - 3. The compiler is able to dynamically resolve `this` to the correct type and hence call the correct overloaded visit method.

## Example
```java
interface Shape {
    void move(int x, int y);
    void draw();
    void accept(Visitor v);
}

class Dot implements Shape {
    public void move(int x, int y) { }
    public void draw() { }

    public void accept(Visitor v) {
        // Compiler knows that `this` is a `Dot`.
        // visit(Dot d) will be called.
        v.visit(this);
    }
}

class Circle implements Shape {
    public void move(int x, int y) { }
    public void draw() { }

    public void accept(Visitor v) {
        // Compiler knows that `this` is a `Circle`.
        // visit(Circle c) will be called.
        v.visit(this);
    }
}

class Rectangle implements Shape {
    public void move(int x, int y) { }
    public void draw() { }

    public void accept(Visitor v) {
        // Compiler knows that `this` is a `Rectangle`.
        // visit(Rectangle r) will be called.
        v.visit(this);
    }
}

interface Visitor {
    void visit(Dot d);
    void visit(Circle c);
    void visit(Rectangle r);
    // and so on for other Shape implementations...
}

class XMLExportVisitor implements Visitor {
    public void visit(Dot d) {
        System.out.println("Calling visit(Dot d)");
    }

    public void visit(Circle c) {
        System.out.println("Calling visit(Circle c)");
    }

    public void visit(Rectangle r) {
        System.out.println("Calling visit(Rectangle r)");
    }
}

public class Test {
    private static List<Shape> allShapes;

    public static void main(String[] args) {
        allShapes = new ArrayList<>();
        allShapes.add(new Rectangle());
        allShapes.add(new Dot());

        XMLExportVisitor exportVisitor = new XMLExportVisitor();

        // This will not compile as we don't have a visit(Shape s) method.
        // exportVisitor.visit(allShapes.get(0));

        // This is the double dispatch call.
        // Shape takes the visitor, which then calls the method on the visitor
        // giving itself to the visitor as an argument.
        for (Shape shape : allShapes) {
            shape.accept(exportVisitor);
        }
    }
}
```
