# Template Method Pattern
## Overview
Template method is a **behavioural pattern** that defines the **skeleton of an algorithm in the superclass** but lets **subclasses override specific steps** of the algorithm **without changing its structure**.

## Advantages
- Let clients override only certain parts of algorithms, minimising effects by changes to other parts of the algorithm.
- Pull duplicate code into a superclass.

## Disadvantages
- Some clients may be limited by the provided skeleton of an algorithm.
- Might violate **Liskov Substitution Principle** by suppressing a default step implementation via a subclass.
- Hard to maintain with more steps in the template method.

## Applicability
- When you want to let clients **extend only particular steps of an algorithm**, but not the whole algorithm or its structure.
- When you have several classes that contain **almost identical algorithms** with **some minor differences**. Such an algorithm should be turned into a template method and the common steps pulled up the base abstract class.

## Comparison with Strategy Pattern
- Template method is based on **inheritance**, with subclasses altering algorithm steps. Strategy is based on **composition** by supplying objects with different strategies.
- Template method works at the **class level**, so is **static**. Strategy pattern works on the **object level**, so is **dynamic** and can be changed at runtime. The subclasses can then implement the steps that differ.

## Example
```java
abstract class Game {
    // Template method
    public final void play() {
        initialise();
        startPlay();
        endPlay();

        if (isGameWon()) {
            showGameWonMessage();
        }
    }

    // These 3 abstract methods MUST be implemented by subclasses.
    protected abstract void initialise();
    protected abstract void startPlay();
    protected abstract void endPlay();

    // Hook method. 
    // This is a (mostly) empty default implementation.
    // Subclass can override, but is not required to.
    boolean isGameWon() {
        return true;
    }

    // Concrete method.
    // Subclasses cannot override this method.
    private void showGameWonMessage() {
        System.out.println("You win!");
    }
}

class Mario extends Game {
    @Override
    protected void initialise() {
        System.out.println("Mario Game Initialised");
    }

    @Override
    protected void startPlay() {
        System.out.println("Playing Mario Game");
    }

    @Override
    protected void endPlay() {
        System.out.println("Ending Mario Game");
    }
}

class Tetris extends Game {
    @Override
    protected void initialise() {
        System.out.println("Tetris Game Initialised");
    }

    @Override
    protected void startPlay() {
        System.out.println("Playing Tetris Game");
    }

    @Override
    protected void endPlay() {
        System.out.println("Ending Tetris Game");
    }
    
    @Override
    protected boolean isGameWon() {
        return false;
    }
}

public class Test {
    public static void main(String[] args) {
        Game marioGame = new Mario();
        marioGame.play();

        Game tetrisGame = new Tetris();
        tetrisGame.play();
    }
}

```