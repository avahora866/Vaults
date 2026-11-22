---
tags:
  - DesignPattern
---
The **Builder pattern** separates the construction of a complex object from its representation, so the same construction process can create different representations.

---

# When to Use It
Use the Builder pattern when:
- You need to build an object step by step.
- The object has many optional parts or configurations.
- You want to decouple construction logic from the object’s representation.
- You want immutable objects with controlled construction.

---

# Key Components

| Component       | Role                                                                 |
|-----------------|----------------------------------------------------------------------|
| **Builder**     | Declares methods to build parts of a product                         |
| **ConcreteBuilder** | Implements the steps defined in Builder to build a specific product |
| **Product**     | The complex object being built                                       |
| **Director**    | (Optional) Orchestrates the build steps in a defined sequence        |
| **Client**      | Creates the builder and optionally uses the director                 |

---

# UML-Like Structure (Text Representation)

```
Client --> Director --> Builder
                          |
                          v
                  ConcreteBuilder --> Product
```

---

# Java Example

## Product

```java
class House {
    private String foundation;
    private String walls;
    private String roof;

    public void setFoundation(String foundation) { this.foundation = foundation; }
    public void setWalls(String walls) { this.walls = walls; }
    public void setRoof(String roof) { this.roof = roof; }

    public void describe() {
        System.out.println("House with foundation: " + foundation +
                           ", walls: " + walls +
                           ", roof: " + roof);
    }
}
```

## Builder Interface

```java
interface HouseBuilder {
    void buildFoundation();
    void buildWalls();
    void buildRoof();
    House getResult();
}
```

## Concrete Builder

```java
class WoodenHouseBuilder implements HouseBuilder {
    private House house = new House();

    public void buildFoundation() {
        house.setFoundation("Wooden foundation");
    }

    public void buildWalls() {
        house.setWalls("Wooden walls");
    }

    public void buildRoof() {
        house.setRoof("Wooden roof");
    }

    public House getResult() {
        return house;
    }
}
```

## Director

```java
class Director {
    private HouseBuilder builder;

    public Director(HouseBuilder builder) {
        this.builder = builder;
    }

    public void constructHouse() {
        builder.buildFoundation();
        builder.buildWalls();
        builder.buildRoof();
    }
}
```

## Client Code

```java
public class Main {
    public static void main(String[] args) {
        HouseBuilder builder = new WoodenHouseBuilder();
        Director director = new Director(builder);
        director.constructHouse();
        House house = builder.getResult();
        house.describe();
    }
}
```

---

# Advantages
- Fine control over the construction process
- Supports building different representations of the same product
- Cleaner code for complex object creation
- Can enforce immutability by constructing fully before returning

---

# Disadvantages
- Adds extra classes and complexity
- Not worth using for simple object construction

---

# Alternative Use: Fluent Interface Builder (Common in Java)

For example, in building immutable data objects:

```java
class Person {
    private final String name;
    private final int age;

    private Person(Builder builder) {
        this.name = builder.name;
        this.age = builder.age;
    }

    static class Builder {
        private String name;
        private int age;

        public Builder setName(String name) {
            this.name = name;
            return this;
        }

        public Builder setAge(int age) {
            this.age = age;
            return this;
        }

        public Person build() {
            return new Person(this);
        }
    }
}
```

## Usage:
```java
Person p = new Person.Builder()
              .setName("Alice")
              .setAge(30)
              .build();
```

---

# Comparison

| Pattern          | Use Case                                     |
|------------------|----------------------------------------------|
| Factory Method   | Create single objects without specifying class |
| Abstract Factory | Create families of related objects            |
| **Builder**      | Construct complex objects step by step        |
| Prototype        | Copy an existing object                      |
