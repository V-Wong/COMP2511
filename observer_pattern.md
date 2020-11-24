# Observer Pattern
## Overview
Observer is a **behavioural pattern** that lets you define a **subscription mechanism** to **notify multiple objects** about any **events that happen to an observed object**.

## Advantages
- **Avoids tight coupling** between subject and observers as subjects do not need to know about implementation details of the observers.
- Allows **dynamic** registration and deregistration.

## Disadvantages
- Change in subject may result in chain of updates to observers and in turn their dependent objects - resulting in **complex update behaviour**.
- Careful attention must be paid to manage such dependencies.

## Applicability
- When changes to the **state of one object may require changing other objects**, and the actual set of objects is unknown beforehand or changes dynamically.
- Highly applicable to **GUI programming** where changes to some object must notify such a change to its corresponding GUI object.

## Push vs Pull Implementation
Push:
- ```update(data1, data2, ...)```
- Subject passes down only the necessary information to observers.

Pull:
- ```update(this)```
- Subject passes a reference of itself down to observers.
- Observers are then responsibile for grabbing required data.

## Example
```java
// A subject interface that represents the class to be observed.
// This is usually implemented by a more complex class with other behaviours.
public interface Subject {
    public void subscribe(Observer observer);
    public void unsubscribe(Observer observer)
    public void notify();
}

// An observer that executes some behaviour when notified by the subject.
// This is often a state change in the subject.
public interface Observer {
    public void update(Subject subject);
}

public class Thermometer implements Subject {
    List<Observer> observers = new ArrayList<Observer>();
    double temperature = 0.0;

    @Override
    public void subscribe(Observer observer) {
        if (!observers.contains(observer))
            observers.add(observer);
    }

    @Override
    public void unsubscribe(Obvserver observer) {
        observers.remove(observer);
    }

    @Override
    public void notify() {
        for (Observer observer : observers)
            observer.update(this);
    }

    public double setTemperature(double temperature) {
        this.temperature = temperature;
        notify();
    }

public class DisplayUSA implements Observer {
    @Override
    public void update(Subject subject) {
        if (subject instanceof Thermometer) update((Thermometer)subject);
    }

    public void update(Thermometer subject) {
        display(temperatureInC);
    }

    public void update(Hydrometer subject) { ... }

    // And other update methods for other class instances of the subject...

    public void display(int temperatureInC) {
        System.out.println("From DisplayUSA: Temperature is " + convertToF(temperatureInC) + " C");
    }

    public double convertToF(double temperatureInC) {
        return temperatureInC * (9.0 / 5.0) + 32;
    }
}
```