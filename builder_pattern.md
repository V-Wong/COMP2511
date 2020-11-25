# Builder Pattern
## Overview
Builder is a **creational pattern** that lets you **construct complex objects step by step**. It allows you to produce **different types and representations** of an object using the **same construction code**.

## Advantages
- Can construct objects **step-by-step**, defer construction steps or run steps recursively.
- Can **reuse the same construction code** when building various representations of products.
- Satisfies **single reponsibility principle** as complex construction code is isolated from business logic of product. We can **remove complex logic and temporary variables** from the main code, isolating it entirely within the builder class.

## Disadvantages
- Overall **complexity of code increases** since pattern requires creating multiple new classes.

## Applicability
- Get rid of **telescopic constructors**. This is when we have a long constructor with many optional parameters. Since calling such a constructor is inconvenient, we overload and create shorter versions with fewer parameters. These constructors still refer to the main one, passing some default values into any omitted parameters. We replace this with the builder pattern, by building objects step by step.

- Create **different representations** of products. Can be applied when construction of various representations of products involve **similar steps that differ only in details**. Base builder interface defines construction steps, and concrete builders implement these steps. The director class then guides the order of construction.

- Construct **composite trees** or other **complex objects**. Builder pattern allows steps to be called **recursively**, which is useful for constructing object trees.

## Relations with Other Patterns
- Many designs start with **Factory Method** and evolve towards Abstract Factory or Builder.
- Builder is often used to create complex **composite trees** as construction steps can be programmed to work **recursively**.

## Example
```java
// Builder interface that specifies the methods for creating 
// the different parts of the product objects.
// Note that the final method to get the resulting object is not defined here.
// The method is instead implemented in the implementing classes, 
// and the return type is determined in the implementing class.
// Different Builder class implementations can hence return different type of objects.
public interface Builder {
    public void reset();
    public void setCarType(CarType type);
    public void setSeats(int seats);
    public void setEngine(Engine engine);
    public void setTransmission(Transmission transmission);
    public void setTripComputer(TripComputer tripComputer);
    public void setGPSNavigator(GPSNavigator gpsNavigator);
}

// Car product is complex with many subproducts and requires extensive configuration.
public class Car {
    private final CarType carType;
    private final int seats;
    private final Engine engine;
    private final Transmission transmission;
    private final TripComputer tripComputer;
    private final GPSNavigator gpsNavigator;
    private double fuel = 0;

    public Car(CarType carType, int seats, Engine engine, Transmission transmission,
               TripComputer tripComputer, GPSNavigator gpsNavigator) {
        this.carType = carType;
        this.seats = seats;
        this.engine = engine;
        this.transmission = transmission;
        this.tripComputer = tripComputer;
        if (this.tripComputer != null)
            this.tripComputer.setCar(this);
        this.gpsNavigator = gpsNavigator;
    }

    public CarType getCarType() {
        return carType;
    }
}

// Manual is also complex with many subcomponents.
// Manual is used here to show here Builder pattern can return different types of objects.
public class Manual {
    private final CarType carType;
    private final int seats;
    private final Engine engine;
    private final Transmission transmission;
    private final TripComputer tripComputer;
    private final GPSNavigator gpsNavigator;

    public Manual(CarType carType, int seats, Engine engine, Transmission transmission,
                  TripComputer tripComputer, GPSNavigator gpsNavigator) {
        this.carType = carType;
        this.seats = seats;
        this.engine = engine;
        this.transmission = transmission;
        this.tripComputer = tripComputer;
        this.gpsNavigator = gpsNavigator;
    }

    public String print() {
        String info = "";
        info += "Type of car: " + carType + "\n";
        info += "Count of seats: " + seats + "\n";
        info += "Engine: volume - " + engine.getVolume() + "; mileage - " + engine.getMileage() + "\n";
        info += "Transmission: " + transmission + "\n";
        if (this.tripComputer != null) {
            info += "Trip Computer: Functional" + "\n";
        } else {
            info += "Trip Computer: N/A" + "\n";
        }
        if (this.gpsNavigator != null) {
            info += "GPS Navigator: Functional" + "\n";
        } else {
            info += "GPS Navigator: N/A" + "\n";
        }
        return info;
    }
}

// Concrete builder class that implements the builder interface
// and provides specific implementation of building steps.
// A program may have several variations of builders, with varying implementations.
public class CarBuilder implements Builder {
    private CarType type;
    private int seats;
    private Engine engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;

    @Override
    public void reset() {
        // reset all instance variables.
    }

    public void setCarType(CarType type) {
        this.type = type;
    }

    @Override
    public void setSeats(int seats) {
        this.seats = seats;
    }

    @Override
    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    @Override
    public void setTransmission(Transmission transmission) {
        this.transmission = transmission;
    }

    @Override
    public void setTripComputer(TripComputer tripComputer) {
        this.tripComputer = tripComputer;
    }

    @Override
    public void setGPSNavigator(GPSNavigator gpsNavigator) {
        this.gpsNavigator = gpsNavigator;
    }

    public Car getResult() {
        return new Car(type, seats, engine, transmission, tripComputer, gpsNavigator);
    }
}

// Builder pattern allows construction of objects that don't
// follow a common interface.
public class CarManualBuilder implements Builder{
    private CarType type;
    private int seats;
    private Engine engine;
    private Transmission transmission;
    private TripComputer tripComputer;
    private GPSNavigator gpsNavigator;

    @Override
    public void reset() {
        // reset all instance variables.
    }

    @Override
    public void setCarType(CarType type) {
        this.type = type;
    }

    @Override
    public void setSeats(int seats) {
        this.seats = seats;
    }

    @Override
    public void setEngine(Engine engine) {
        this.engine = engine;
    }

    @Override
    public void setTransmission(Transmission transmission) {
        this.transmission = transmission;
    }

    @Override
    public void setTripComputer(TripComputer tripComputer) {
        this.tripComputer = tripComputer;
    }

    @Override
    public void setGPSNavigator(GPSNavigator gpsNavigator) {
        this.gpsNavigator = gpsNavigator;
    }

    public Manual getResult() {
        return new Manual(type, seats, engine, transmission, tripComputer, gpsNavigator);
    }
}

// Director is responsible for executing building steps in a particular sequence.
// It is optional as clients can use builders directly.
// It is helpful when producing products according to a specific configuration.
// If a more fine tuned object is required, the client should skip the director
// and use the builders directly.
public class Director {
    // Note that each method takes in a base Builder, not CarBuilder or ManualBuilder specifically.
    // This allows the client code to alter final type of assembled product.

    // We could have alternatively made the director take in the builder take
    // in the builder during construction, and hence remove the builder argument
    // from each method below.

    public void constructSportsCar(Builder builder) {
        builder.setCarType(CarType.SPORTS_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(3.0, 0));
        builder.setTransmission(Transmission.SEMI_AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructCityCar(Builder builder) {
        builder.setCarType(CarType.CITY_CAR);
        builder.setSeats(2);
        builder.setEngine(new Engine(1.2, 0));
        builder.setTransmission(Transmission.AUTOMATIC);
        builder.setTripComputer(new TripComputer());
        builder.setGPSNavigator(new GPSNavigator());
    }

    public void constructSUV(Builder builder) {
        builder.setCarType(CarType.SUV);
        builder.setSeats(4);
        builder.setEngine(new Engine(2.5, 0));
        builder.setTransmission(Transmission.MANUAL);
        builder.setGPSNavigator(new GPSNavigator());
    }
}

public class Test {
    public static void buildCarUsingDirector() {
        Director director = new Director();

        // Director gets the concrete builder object from the client
        // That's because client code knows which builder to use to get a specific product.
        CarBuilder carBuilder = new CarBuilder();
        director.constructSportsCar(carBuilder);

        // The final product is often retrieved from a builder object, since
        // Director is not aware and not dependent on concrete builders and products.
        Car car = carBuilder.getResult();
        System.out.println("Car built:\n" + car.getCarType());
    }

    public static void buildCarDirectlyUsingBuilder() {
        CarBuilder carBuilder = new CarBuilder();

        // More specific configurations often require the client
        // to skip the director and use the builder directly.
        carBuilder.reset();
        carBuilder.setCarType(CarType.OTHER_CARD);
        carBuilder.setSeats(4);
        carBuilder.setEngine(new Engine(5.0, 0));
        carBuilder.setTransmission(Transmission.SEMI_AUTOMATIC);
        carBuilder.setTripComputer(new TripComputer());
        carBuilder.setGPSNavigator(new GPSNavigator());

        Car specialCar = carBuilder.getResult();
        System.out.println("Car built:\n" + specialCar.getCarType());
    }

    public static void buildManual() {
        Director director = new Director();

        CarManualBuilder manualBuilder = new CarManualBuilder();

        // Director may know several building recipes.
        director.constructSportsCar(manualBuilder);
        Manual carManual = manualBuilder.getResult();
        System.out.println("\nCar manual built:\n" + carManual.print());
    }

    public static void main(String[] args) {
        buildCarUsingDirector();
        buildCarDirectlyUsingBuilder();
        buildManual();
    }
}
 
```