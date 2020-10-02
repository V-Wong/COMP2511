# Principle of Least Knowledge
## Overview
A method in an object should only invoke methods of:
- The object itself.
- The object passed in as a parameter to the method.
- Objects instantiated within the method.
- Any component objects.
- **And not those of objects returned by a method**.

THe principle of least knowledge helps us design **loosely-coupled** systems so that **changes** to one part of the system **does not cascade to other parts** of the system.

We try to prevent calls such as:

```java
    o.get(name).get(thing).remove(node)
```


## Rule 1
A method M in an object O can call any other method within O itself.

```java
public class M {
    public void methodM() {
        this.methodN(); // calling another method of same class
    }

    public void methodN() {
        // do something
    }
}
```

## Rule 2
A method M in an object	O can call on any methods of parameters	passed to the method M.
```java
public class O {
    public void M(Friend f) {
        // Invoking a method on a parameter passed to the method is legal
        f.N();
    }
}

public class Friend {
    public void N() {
        // do something
}
```

## Rule 3
A method M can call a method N of another object, if that object is instantiated within the method M.

```java
public class O {
    public void M() {
        Friend f = new Friend();
        // Invoking a method on an object created within the method is legal
        f.N();
    }
}

public class Friend {
    public void N() {
        // do something
    }
}
```

## Rule 4
Any method M in an object O can call on any methods of any type of object that is a direct component of O.

```java
public class O {
    public Friend instanceVar = new Friend();
    public void M4() {
        // Any method can access the methods of the friend class F 
        // through the instance variable "instanceVar"
        instanceVar.N();
    }
}

public class Friend {
    public void N() {
    // do something
    }
}
```