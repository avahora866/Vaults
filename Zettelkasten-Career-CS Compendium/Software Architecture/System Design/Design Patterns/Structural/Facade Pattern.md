---
tags:
  - DesignPattern
---
The **Facade** pattern is a **structural** design pattern that provides a **unified interface** to a set of interfaces in a subsystem. It defines a higher-level interface that makes the subsystem **easier to use**.

# Intent

- Simplify interaction with a complex subsystem.
- Provide a single entry point for clients.
- Reduce coupling between clients and complex systems.

# Motivation

Imagine a video conversion library that involves classes like `FileReader`, `VideoDecoder`, `AudioExtractor`, `BitrateController`, and `FileWriter`. If a user wants to convert a video from one format to another, they'd have to manually coordinate all these components — which is overwhelming.

The **Facade** pattern solves this by introducing a `VideoConverter` class that exposes a single method `convert(String filename, String format)`, hiding the internal complexity.

# Structure

```plaintext
         Client
           |
       +---------+
       | Facade  |   <-- provides a simple interface
       +---------+
        /   |   \
Subsystem Subsystem Subsystem
   A        B         C
```

## Participants

- **Facade**: Provides a high-level interface that makes the subsystem easier to use.
- **Subsystem classes**: Handle the actual work but don’t know about the facade. They operate independently.
- **Client**: Uses the facade instead of interacting directly with the subsystems.

# Java Example

Let's simulate a **home theater system** that includes multiple subsystems: amplifier, DVD player, projector, and lights.

## Step 1: Subsystem Classes

```java
class Amplifier {
    void on() { System.out.println("Amplifier ON"); }
    void setVolume(int level) { System.out.println("Amplifier volume set to " + level); }
}

class DVDPlayer {
    void on() { System.out.println("DVD Player ON"); }
    void play(String movie) { System.out.println("Playing movie: " + movie); }
}

class Projector {
    void on() { System.out.println("Projector ON"); }
    void wideScreenMode() { System.out.println("Projector in widescreen mode"); }
}

class TheaterLights {
    void dim(int level) { System.out.println("Lights dimmed to " + level + "%"); }
}
```

## Step 2: Facade Class

```java
class HomeTheaterFacade {
    private Amplifier amp;
    private DVDPlayer dvd;
    private Projector projector;
    private TheaterLights lights;

    public HomeTheaterFacade(Amplifier amp, DVDPlayer dvd, Projector projector, TheaterLights lights) {
        this.amp = amp;
        this.dvd = dvd;
        this.projector = projector;
        this.lights = lights;
    }

    public void watchMovie(String movie) {
        System.out.println("Get ready to watch a movie...");
        lights.dim(10);
        projector.on();
        projector.wideScreenMode();
        amp.on();
        amp.setVolume(5);
        dvd.on();
        dvd.play(movie);
    }
}
```

## Step 3: Client Code

```java
public class FacadePatternDemo {
    public static void main(String[] args) {
        Amplifier amp = new Amplifier();
        DVDPlayer dvd = new DVDPlayer();
        Projector projector = new Projector();
        TheaterLights lights = new TheaterLights();

        HomeTheaterFacade homeTheater = new HomeTheaterFacade(amp, dvd, projector, lights);
        homeTheater.watchMovie("Inception");
    }
}
```

## Output

```
Get ready to watch a movie...
Lights dimmed to 10%
Projector ON
Projector in widescreen mode
Amplifier ON
Amplifier volume set to 5
DVD Player ON
Playing movie: Inception
```

# Benefits

- **Simplifies client usage**: Clients don’t need to understand or interact with subsystem internals.
- **Decouples**: Reduces coupling between clients and subsystems.
- **Encapsulates**: Provides a clear point of entry and hides complexity.

# Drawbacks

- **One-size-fits-all interface**: May limit flexibility for advanced clients who need fine-grained control.
- **Potentially a god object**: If not designed carefully, the facade can grow too large and become hard to maintain.

# When to Use

- When you want to provide a simple interface to a complex subsystem.
- When you want to layer your system, with facades between each layer.
- When you want to decouple clients from the implementation details of a library or framework.

# Real-World Analogies

- **Remote control**: You press one button, and it starts the TV, turns on the sound system, and adjusts the lights.
- **Hotel receptionist**: One interface to multiple services (room service, housekeeping, taxi booking).

# Related Patterns

- **Adapter**: Makes an interface compatible; Facade simplifies an interface.
- **Mediator**: Centralizes communication between components; Facade centralizes access.
- **Abstract Factory**: Can be used behind a facade to create subsystem objects.