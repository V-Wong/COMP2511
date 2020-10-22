# Refactoring Techniques
## Overview
**Refactoring** is the process of **restructuring** software to make it easier to understand and modify **without changing external behaviour**.

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
