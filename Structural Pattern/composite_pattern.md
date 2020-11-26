# Composite Pattern
## Overview
Composite is a **structural pattern** that lets you compose objects into **tree structures** and then work with these structures as if they were **individual objects**.

## Advantages
- Work with complex tree structures more conveniently using polymorphism and recursion.
- Satsifies **open/closed principle** as new element types can be introduced and added to object trees without breaking existing code.

## Applicability
- When you have to implement a tree-like object structure.
- When you want to tree simple and complex elements uniformly.

## Example
```java
// The interface that must be implemented
// by both composite and leaf nodes in
// our object tree.
public interface Graphic {
    public void move(int x, int y);
    public void draw();
}

// A leaf node in our object tree.
public class Dot implements Graphic {
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
public class CompoundGraphic implements Graphic {
    private List<Graphic> children;

    public CompoundGraphi(List<Graphic> children) { ... }

    public void add(Graphic child) {
        this.children.add(child);
    }

    public void remove(Graphic child) {
        this.children.remove(child);
    }

    public void move(int x, int y) {
        for (Graphic child : children)
            child.move(x, y);
    }

    public void draw() {
        for (Graphic child : children)
            child.draw();
    }
}
```