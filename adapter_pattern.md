# Adapter Pattern
## Overview
Adapter is a **structural pattern** that allows objects with **incompatible interfaces to collaborate**.

## Advantages
- Satisfies **single responsibility principle** as you can separate the the interface or data conversion code from the primary business logic of the program.
- Satisfies **open/closed principle** as new adapters can be introduced without breaking existing client code.

## Disadvantages
- Increased **complexity** with the introduction of new interfaces and classes. Sometimes **changing the service class** to match the rest of the code is simpler.

## Applicability
- When you want to use some existing class, but its **interface isn't compatible** with the rest of your code. This is essentially a **middle-layer** class that **translates** between your code and a legacy class, a 3rd party class or any other class.
- When you want to **reuse several existing subclasses** that **lack some common functionality** that can't be added to the superclass. You can wrap the objects with missing features inside an adapter. This is very similar to the decorator pattern.

## Example
```java
interface LightningPhone {
    public void recharge();
    public void useLightning();
}

interface MicroUsbPhone {
    public void recharge();
    public void useMicroUsb();
}

class Iphone implements LightningPhone {
    private boolean connector;

    @Override
    public void useLightning() {
        connector = true;
        System.out.println("Lightning connected");
    }

    @Override
    public void recharge() {
        if (connector) {
            System.out.println("Recharge started");
            System.out.println("Recharge finished");
        } else {
            System.out.println("Connect lightning first");
        }
    }
}

class Android implements MicroUsbPhone {
    private boolean connector;

    @Override
    public void useMicroUsb() {
        connector = true;
        System.out.println("MicroUsb connected");
    }

    @Override
    public void recharge() {
        if (connector) {
            System.out.println("Recharge started");
            System.out.println("Recharge finished");
        } else {
            System.out.println("Connect MicroUsb first");
        }
    }
}

// Adapter class:
// Wraps a LightningPhone object.
// Implements the MicroUsbPhone interface.
class LightningToMicroUsbAdapter implements MicroUsbPhone {
    private final LightningPhone lightningPhone;

    public LightningToMicroUsbAdapter(LightningPhone lightningPhone) {
        this.lightningPhone = lightningPhone;
    }

    @Override
    public void useMicroUsb() {
        // This is a wrapped method.
        // Often times, other more complex code or conversion logic is ran 
        // before delegating back to the original unwrapped object.
        System.out.println("MicroUsb connected");
        lightningPhone.useLightning();
    }

    @Override
    public void recharge() {
        lightningPhone.recharge();
    }
}

public class Test {
    static void rechargeMicroUsbPhone(MicroUsbPhone phone) {
        phone.useMicroUsb();
        phone.recharge();
    }

    static void rechargeLightningPhone(LightningPhone phone) {
        phone.useLightning();
        phone.recharge();
    }

    public static void main(String[] args) {
        Android android = new Android();
        Iphone iPhone = new Iphone();

        System.out.println("Recharging android with MicroUsb");
        rechargeMicroUsbPhone(android);

        System.out.println("Recharging iPhone with Lightning");
        rechargeLightningPhone(iPhone);

        // This is a compilation error as the interfaces are incompatible.
        // rechargeMicroUsbPhone(iPhone);

        // We can use the adapter to convert the LightningPhone interface into 
        // a MicroUsbPhone interface.
        System.out.println("Recharging iPhone with MicroUsb");
        rechargeMicroUsbPhone(new LightningToMicroUsbAdapter(iPhone));
    }
}
```