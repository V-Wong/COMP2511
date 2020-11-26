# Singleton Pattern
## Overview
Singleton is a **creational pattern** that lets you ensure that a class has **only one instance**, while providing a **global access point** to this instance.

## Disadvantages
- Violates **single responsibility principle** as pattern solves two problems at the same time.
- Can **mask bad design** when components of program know too much about each other.

## Applicability
- When a class in our program should have **only one instance**. This may be a **database**, **logger**, etc.

## Example
```java
class Database {
    private static Database instance;

    private Database() {
        // do actual logic for connecting to a DB
    }

    public static synchronized Database getInstance() {
        if (instance == null) {
            instance = new Database();
        }

        return instance;
    }

    // A singleton should define some business logic
    // which can be executed on its instance.
    public void query(String sql) {
        // query the DB.
    }
}

public class Test {
    public static void main(String[] args) {
        Database db1 = Database.getInstance();
        db1.query("test");

        Database db2 = Database.getInstance();
        db2.query("test");

        // Both db1 and db2 should contain the same object.
        System.out.println(db1.equals(db2));
    }
}
```