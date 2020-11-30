# Refactoring Techniques
## Overview
**Refactoring** is the process of **restructuring** software to make it easier to understand and modify **without changing external behaviour**.

## Refactoring Techniques
|Technique Name|Description|
|-|-|
|Extract Method|Group code fragments together in a method.|
|Move Method|Move methods to the class most appropriate.|
|Replace Temp With Query| Remove temp variables with method calls.|
|Replace Conditional With Polymorphism|Override methods in subclasses with specific behaviour to remove class/instance checks.|
|Introduce Parameter Object|Make a new class that holds variables that are often grouped together and passed into functions.|
|Preserve Whole Object|Pass entire objects into functions instead of individual values from the object.|
|Replace Inheritance With Delegation|Hold the superclass object in a private field and delegate the behaviours as required.|
|Push Down Field and Method|Fields and methods should be placed in the subclasses that actually use them instead of the superclass.|
|Extract Subclass|Behaviours only used in special cases by a class should be moved down to a subclass.|

## Extract Method
### Explanation
Methods with more lines are harder to understand. Code fragments that can be **grouped** together should be moved to their **own method**.

### Advantages
- Makes code easier to understand as methods have self evident names.
- Reduce code duplication as these groups of code are often reused.
- Isolate independent parts of code to minimise chance of errors.

### Example
Before:
```java
    public void printOwing() {
        printBanner();

        // These lines can be grouped together
        System.out.println("name: " + name);
        System.out.println("amount: " + getOutStanding());
    }
```

After:
```java
    public void printOwing() {
        printBanner();
        printDetails();
    }

    public void printDetails() {
        System.out.println("name: " + name);
        System.out.println("amount: " + getOutStanding());
    }
```

## Move Method
### Explanation
A method should generally be on the object whose **data it uses most**. When moving such a method to a new class, the method in the original class should be changed to **reference the new method** or be **removed completely**.

### Advantages
- Makes the class more internally coherent.
- Reduces dependencies between classes.

## Replace Temp With Query
### Explanation
The **result of expressions** should be **returned from a method** rather than placed in **temporary variables**.

### Advantages
- If the method is named properly, it is **easier to understand** than the formula in the expression.
- If the expression is reused in **multiple methods**, then we **reduce code duplication**.
- We are less likely to **lose track** of local variables in **longer methods**.

### Disadvantages
- Repeated function calls and recalculations can result in a **performance hit**. But this is often negligible.

### Example
Before:
```java
public double calculateTotal() {
    double basePrice = quantity * itemPrice;

    if (basePrice > 1000) return basePrice * 0.95;
    else return basePrice * 0.98
}
```

After:
```java
public double calculateTotal() {
    if (basePrice > 1000) return basePrice() * 0.95;
    else return basePrice() * 0.98
}

public double basePrice() {
    return quantity * itemPrice;
}
```

## Replace Conditional Logic With Polymorphism
### Explanation
Conditionals are often harder to maintain and create **coupling** between classes. Instead, **subclasses** should be created while **overriding** the method, and **polymorphism** utilised to select the correct method to use at runtime.

### Advantages
- Reduces **coupling** between classes. Minimises **changes** needing to be made **over multiple classes**.
- Keeps classes more **cohesive** as classes no longer have to perform logic **outside their responsibility**. Logic is instead **delegated** to the most appropriate class.

### Example
Before:
```java
public class Rental {
    Movie movie;
    Integer daysRented;

    public double getCharge() {
        double totalAmount = 0;
        int priceCode = movie.getPriceCode();

        // This requires internal knowledge of the Movie class which is unnecessary coupling.
        // Also changes or addition of movie types will require changes in both Rental and Movie classes.
        switch (priceCode) {
            case Movie.REGULAR:
                totalAmount = (daysRented - 2) * 1.5;
                break;
            case Movie.CHILDRENS:
                // more logic
                break;
            case Movie.NEW_RELEASE:
                // more logic
                break;
        }

        return totalAmount;
    }
}

public class Movie {
    public static final int REGULAR = 0;
    public static final int CHILDRENS = 1;
    public static final int NEW_RELEASE = 2;

    private String title;
    public int priceCode;

    public int getPriceCode() {
        return priceCode;
    }
}
```

After:
```java
public class Rental {
    Movie movie;
    Integer daysRented;

    // This method no longer needs to know the structure of the Movie class.
    // Changes or addition of movie types will not require changes here.
    public double getCharge() {
        return movie.getCharge(daysRented);
    }
}

public abstract class Movie {
    private String title;

    // getCharge now needs to know the days rented to perform the calculations.
    public int getCharge(int daysRented);
}

public RegularMovie extends Movie {
    public int getCharge(int daysRented) {
        return (daysRented - 2) * 1.5;
    }
}

public ChildrensMovie extends Movie {
    public int getCharge(int daysRented) {
        // specific logic
    }
}

public NewReleaseMovie extends Movie {
    public int getCharge(int daysRented) {
        // specific logic
    }
}
```

Note: this has a different problem in that movies cannot change classification during its lifetime. A fix for this would be instead to utilise composition:
- Create a Price interface with a single getCharge() method.
- Implement this interface for each of RegularMovie, ChildrensMovie and NewReleaseMovie.
- Hold an object of this interface in a field inside the Movie class. Usually the constructor would require such an object.
- The Movie class can then delegate price calculation to the held object.
- The Movie class can also change the specific price object with one of a different type during runtime by using an appropriate setter function.

## Introduce Parameter Object
### Explanation
**Identical groups of parameters** scattered throughout the code is a form of **code duplication**. These parameters should be **consolidated into a single class**.

### Advantages
- Reduces **code duplication**.
- Prevents methods from having **long list of parameters** which can be hard to comprehend and use.

### Example
Before:
```java
abstract class Customer {
    public Integer amountInvoicedIn(Date start, Date end);
    public Integer amountReceivedIn(Date start, Date end);
    public Integer amountOverdueIn(Date start, Date end);
}
```

After:
```java
abstract class Customer {
    public Integer amountInvoicedIn(DateRange range);
    public Integer amountReceivedIn(DateRange range);
    public Integer amountOverdueIn(DateRange range);
}
```

## Preseve Whole Object
### Explanation
Essentially the same as **Introduce Parameter Object**. Instead of extracting extracting values from an object and passing into a function, **pass the whole object into the function**. 

### Advantages
- A **single object with a comprehensible name** is seen instead of a list of unrelated parameter names.
- If the function needs **more values from the object**, **only the function body needs to be changed**. Changes do not need to be made at each place the function is called.

### Disadvantages
- Potentially **less flexible** as the function is now limited to objects of the **required class**.

### Examples
Before:
```java
Integer low = daysTempRange.getLow();
Integer high = daysTempRange.getHigh();
Boolean withinPlan = plan.withinRange(low, high);
```

After:
```java
Boolean withinPlan = plan.withinRange(daysTempRange);
```

## Replace Inheritance with Delegation
### Explanation
**Refused bequest** is the code smell where subclasses **use only some** of the methods and properties inherited from its parents. The often arises from the **incorrect use of inheritance**. In such cases, a **field should be added** to the class to **hold its superclass object**. Methods should then be **delgated to the superclass object**.

### Advantages
- Class doesn't contain **unneeded methods** inherited from superclass.
- Can help satisfy **Liskov Substitution Principle** if the original inheritance design was not suitable.

### Disadvantages
- May require writing a lot of simple delegating methods.

### Example
Before:
```java
class Stack<E> extends Vector<E> {
    // Inherit all Vector methods.

    // Add additional Stack specific methods.
}
```

After:
```java
class Stack<E> {
    private Vector<E> v;

    public Stack() {
        this.v = new Vector<E>();
    }

    public boolean isEmpty() {
        return v.isEmpty();
    }

    public void push(E e) {
        v.add(e);
    }

    public E pop() {
        return v.remove(v.size() - 1);
    }

    // and other delegated methods as necessary...
}
```

## Push Down Field and Method
### Explanation
If a field or method in a superclass is **only used in some subclasses**, then they should be **moved to the subclasses**. 

### Advantages
- Improves **internal class coherency** as fields and methods are located where they are **actually used**.

## Extract Subclass
### Explanation
If a class has features that are **only used in certain case**s, then these features should be **moved into a subclass** for the special case. This is essentially an application of **Push Down Field and Method**.

Note: this technique can run into the usual problems faced by inheritance. In such cases, **delegation and strategy pattern** are viable alternatives.