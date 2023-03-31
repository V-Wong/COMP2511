# COMP2511 - Object Oriented Programming
My study notes for the various objected oriented refactoring techniques and design patterns. Includes code samples as well as written text explanations.

Note: [Refactoring Guru](https://refactoring.guru/refactoring) is likely a much better resource than these notes.

## General
- [Domain Modelling](domain_modelling.md)
- [Refactoring Techniques](refactoring_techniques.md)

## Design Principles
- [Principle of Least Knowledge (Law of Demeter)](law_of_demeter.md)

## Design Patterns
### Structural Patterns
Structural patterns explain how to assemble objects and classes into larger structures, while keeping the structures flexible and efficient.

|Pattern|Description|
|-------|-------|
|[Composite Pattern](structural_patterns/composite_pattern.md)|Compose objects into **tree structures** and then work with these structures as if they were **individual objects**.|
|[Adapter Pattern](structural_patterns/adapter_pattern.md)|Allows objects with **incompatible interfaces** to collaborate.
|[Decorator Pattern](structural_patterns/decorator_pattern.md)|**Dynamically attach new behaviours** to objects by placing these objects **inside special wrapper objects** that contain the behaviours.

### Behavioural Patterns
Behavioral patterns take care of effective communication and the assignment of responsibilities between objects.

|Pattern|Description|
|-------|-------|
|[Strategy Pattern](behavioural_patterns/strategy_pattern.md)|Extract **varying behaviours** into separate classes and allow these to be changed in a context object **dynamically**.|
|[State Pattern](behavioural_patterns/state_pattern.md)|Allows objects to **alter its behaviour** when its **internal state changes**.|
|[Observer Pattern](behavioural_patterns/observer_pattern.md)|Define a **subscription mechanism** to **notify multiple objects** about any **events that happen to an observed object**.|
|[Template Method Pattern](behavioural_patterns/template_pattern.md)|Define the **skeleton of an algorithm** in the superclass but lets **subclasses override specific steps** of the algorithm **without changing its structure**.|
|[Visitor Pattern](behavioural_patterns/visitor_pattern.md)|**Separate algorithms from the objects** on which they operate. Allow **new behaviours** to be added to **existing classes** without modifying them (mostly).

### Creational Patterns
Creation patterns provide various object creation mechanisms that increase flexibility and code reuse.

|Pattern|Description|
|-------|-------|
|[Factory Method](creational_patterns/factory_pattern.md)|Provide an interface for creating objects in a superclass, but allows subclasses to **alter the type** of objects that will be created.|
|[Abstract Factory Pattern](creational_patterns/abstract_factory_pattern.md)|Produce **familes of related objects** without specifying their concrete classes.|
|[Builder Pattern](creational_patterns/builder_pattern.md)|Construct complex objects **step by step**.|
|[Singleton Pattern](creational_patterns/singleton_pattern.md)|Ensure that a class has **only one instance**, while providing a **global access point** to this instance.|


## Other
- [Method Overriding (Covariance and Contravariance)](method_overriding.md)
