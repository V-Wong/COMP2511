# Composite Pattern
## Overview
Composite is a **structural pattern** that lets you compose objects into **tree structures** and then work with these structures as if they were **individual objects**.

## Advantages
- Work with complex tree structures more conveniently using **polymorphism** and **recursion**.
- Satsifies **open/closed principle** as new element types can be introduced and added to object trees without breaking existing code.

## Applicability
- When you have to implement a tree-like object structure.
- When you want to treat simple and complex elements uniformly.

## Example
```java
// The interface that must be implemented by both composite and leaf nodes in our object tree.
public interface Graphic {
    public void move(int x, int y);
    public void draw();
}

// A leaf node in our object tree.
class Dot implements Graphic {
    private int x;
    private int y;

    public Dot(int x, int y) { ... }

    public void move(x, y) {
        this.x += x;
        this.y += y;
    }

    public void draw() { ... }
}

// A composite node in our object tree.
class CompoundGraphic implements Graphic {
    private List<Graphic> children;

    public CompoundGraphic(List<Graphic> children) { 
        this.childdren = children;
    }

    public void add(Graphic child) {
        children.add(child);
    }

    public void remove(Graphic child) {
        children.remove(child);
    }

    public void move(int x, int y) {
        for (Graphic child : children)
            child.move(x, y);
    }

    // We propagate the draw() call down to each children
    // and then it will propagate down to its children and so on
    // until a leaf node is hit.
    public void draw() {
        for (Graphic child : children)
            child.draw();
    }
}
```
