---
tags:
  - DesignPattern
---
the **Flyweight** pattern is a **structural** design pattern that focuses on **minimizing memory usage** by sharing as much data as possible with similar objects. It’s especially useful when dealing with large numbers of objects that have **shared intrinsic state**.

# Intent

- Share common state across multiple objects to reduce memory footprint.
- Separate **intrinsic** (shared) and **extrinsic** (unique/contextual) state.

# Motivation

Imagine a text editor that displays millions of characters. If each character object stores formatting information like font, size, color, etc., you end up duplicating the same data for every character — wasting memory.

The **Flyweight** pattern allows you to **reuse** objects that share the same intrinsic state (e.g., formatting) and keep only extrinsic state (e.g., position) outside the object.

# Key Concepts

- **Intrinsic State**: Shared state that can be reused. Stored in the flyweight object.
- **Extrinsic State**: Contextual state that varies per use. Passed to the flyweight when needed.

# Structure

```plaintext
    Client
      |
  FlyweightFactory
      |
   Flyweight Pool  <-- shared objects
      |
  +-----------+
  | Flyweight |
  +-----------+
       ^
       |
ConcreteFlyweight (shared)
```

# Java Example

Let’s model a text editor that handles many characters efficiently.

## Step 1: Flyweight Interface

```java
interface Character {
    void display(int x, int y); // x and y are extrinsic (context-specific)
}
```

## Step 2: ConcreteFlyweight (shared characters)

```java
class CharacterA implements Character {
    private final String font; // intrinsic state

    public CharacterA(String font) {
        this.font = font;
    }

    public void display(int x, int y) {
        System.out.println("Character A at (" + x + ", " + y + ") in font " + font);
    }
}

class CharacterB implements Character {
    private final String font;

    public CharacterB(String font) {
        this.font = font;
    }

    public void display(int x, int y) {
        System.out.println("Character B at (" + x + ", " + y + ") in font " + font);
    }
}
```

## Step 3: Flyweight Factory

```java
import java.util.HashMap;
import java.util.Map;

class CharacterFactory {
    private final Map<String, Character> characters = new HashMap<>();

    public Character getCharacter(char c, String font) {
        String key = c + "-" + font;
        if (!characters.containsKey(key)) {
            switch (c) {
                case 'A' -> characters.put(key, new CharacterA(font));
                case 'B' -> characters.put(key, new CharacterB(font));
                // more cases for other characters
            }
        }
        return characters.get(key);
    }
}
```

## Step 4: Client

```java
public class FlyweightPatternDemo {
    public static void main(String[] args) {
        CharacterFactory factory = new CharacterFactory();

        Character charA1 = factory.getCharacter('A', "Arial");
        charA1.display(1, 1);

        Character charA2 = factory.getCharacter('A', "Arial");
        charA2.display(2, 2);

        Character charB = factory.getCharacter('B', "Arial");
        charB.display(3, 3);
    }
}
```

## Output

```
Character A at (1, 1) in font Arial
Character A at (2, 2) in font Arial
Character B at (3, 3) in font Arial
```

## Note

Even though we asked for multiple 'A' characters, the factory only created **one shared instance** for 'A' in Arial font.

# Benefits

- **Memory efficient**: Greatly reduces memory usage when you have many similar objects.
- **Object reuse**: Centralized creation and reuse through a factory.

# Drawbacks

- **Complexity**: Adds complexity with the separation of intrinsic and extrinsic state.
- **Not always beneficial**: Only makes sense when object sharing is meaningful and practical.

# When to Use

- When your application needs to create a large number of similar objects.
- When object identity is not essential.
- When shared objects can be made immutable.

# Real-World Analogies

- **Font rendering**: One glyph object shared for every instance of a character.
- **Particle systems in games**: Reuse texture and behavior definitions.
- **Document editors**: Shared format definitions (fonts, styles) for characters.

# Related Patterns

- **Factory**: Used to manage the creation and reuse of flyweight instances.
- **Composite**: Often used together when modeling hierarchical data with shared parts.
- **Prototype**: Can be used for cloning flyweight objects when needed.