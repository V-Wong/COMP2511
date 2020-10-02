# Domain Modelling
## Unified Modelling Language (UML)
**UML** is a graphical language to model software solutions, application structures, system behaviour and business processes.

## Relationships
### Inheritance
**Inheritance** models a relationship between classes in which one class represents a more general concept (**parent or base class**) and another a more specialised class (**sub-class**).

Inheritance models a **"is-a"** type of relationship.

E.g:
- A savings account is a type of bank account.
- A dog **is-a** type of pet.
- A manager **is-a** type of employee.
- A reactange **is-a** type of 2D shape.

### Association
**Association** is a relationship between two classes, that show that the two classes are:
- Linked to each other, or
- Combined into some kind of **"has-a"** relationship, where on class **"contains"** another class.

E.g. A course offering **has** students.

#### Aggregation
**Aggregation** is a special type of association where the contained item is an element of a collection, but **can exist by itself**. 

E.g. A lecturer is part of a university but exist in their own right as well.

#### Composition
**Composition** is a special type of association where the contained item is an **integral part** of the containing item. 

E.g. A line item is a part of an order but is meaningless on its own.

## Domain Model
**Domain modelling** determines **internal behaviour**. It describes how elements of a system-to-be interact to produce the external behaviour.

A domain model provides a visual representation of the problem domain, through **decomposing the domain** into **key concepts** or **objects** in the real-world and identifying the **relationships between these objects**.

### Noun/Veb Analysis
Nouns in a textual description of the domain are **candidate conceptual classes**.

### Class/Responsibility/Collaborate (CRC)
**Class**: represents a collection of **similar objects**.
**Responsibility**: something that the class **knows** or **does**.
**Collaborator**: another class that a class must **interact with** to fulfil its responsibilities.
