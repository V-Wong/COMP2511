# Method Overriding
## Overview
**Method overriding** allows a subclass to provide a **specific implementation** of a method that is already provided by its base class.

## Rules for Method Overriding
### Covariance
**Covariance** preserves the ordering of types.

The return type in an overriden method should be the same or a sub-type of the return type defined in the super-class.

```java
public class AnimalShelter {
    public Animal getAnimalForAdoption() {
        // do something
    }
}

public class CatShelter extends AnimalShelter {
    @Override
    public Cat getAnimalForAdoption() {
        // do something
    }
}
```

### Contravariance
**Contravariance** reverses the ordering of types.

In the context of OOP, this involves methods in a subclass to have arguments wider than the arguments passed in its parent class.

```java
public class AnimalShelter {
    public Animal putAnimal(Animal someAnimal) {
        // do something
    }
}

public class CatShelter extends AnimalShelter {
    public Cat putAnimal(Object someAnimal) {
        // do something
    }
}
```

Note that Java does not recognise this as overriding. It instead sees it as an unrelated method.
