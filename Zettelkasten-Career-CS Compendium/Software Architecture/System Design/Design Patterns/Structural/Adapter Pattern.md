---
tags:
  - DesignPattern
---
The **Adapter** pattern is a **structural** design pattern that allows incompatible interfaces to work together. It acts as a bridge between two interfaces by converting the interface of a class into another interface that the client expects.

# Intent

- Convert the interface of a class into one that clients expect.
- Let classes work together that couldn't otherwise due to incompatible interfaces.

# Motivation

Imagine you are integrating a third-party library or legacy system into your application. Their class interfaces may not match what your app is expecting. Rather than rewriting existing code, you can use an adapter to make their interface "look like" yours.

# Structure

There are two common forms:

## Object Adapter (uses composition)

- The adapter contains an instance of the adaptee and delegates requests to it.

## Class Adapter (uses inheritance)

- The adapter inherits from both the target interface and the adaptee (only feasible in languages that support multiple inheritance like C++).

## UML Diagram (Object Adapter)

```plaintext
+---------+         +----------------+        +-------------+
|  Client | ----->  |    Target      | <----- |   Adapter   |
+---------+         +----------------+        +-------------+
                             ^                        |
                             |                        |
                        +---------+            +-------------+
                        | Adaptee |            | adaptee ref |
                        +---------+            +-------------+
```

# Participants

- **Client**: Calls methods on the target interface.
- **Target**: The interface expected by the client.
- **Adaptee**: The existing interface that needs adapting.
- **Adapter**: Implements the target interface and translates the call to the adaptee.

# Java Example

Let’s say you have a `MediaPlayer` interface that expects `play(String audioType, String fileName)`, but you also have a third-party `AdvancedMediaPlayer` that can only play `.vlc` or `.mp4` files with separate methods.

## Step 1: Target Interface

```java
interface MediaPlayer {
    void play(String audioType, String fileName);
}
```

## Step 2: Adaptee Interface

```java
interface AdvancedMediaPlayer {
    void playVlc(String fileName);
    void playMp4(String fileName);
}
```

## Step 3: Concrete Adaptees

```java
class VlcPlayer implements AdvancedMediaPlayer {
    public void playVlc(String fileName) {
        System.out.println("Playing vlc file: " + fileName);
    }

    public void playMp4(String fileName) {
        // Do nothing
    }
}

class Mp4Player implements AdvancedMediaPlayer {
    public void playVlc(String fileName) {
        // Do nothing
    }

    public void playMp4(String fileName) {
        System.out.println("Playing mp4 file: " + fileName);
    }
}
```

## Step 4: Adapter

```java
class MediaAdapter implements MediaPlayer {
    private AdvancedMediaPlayer advancedMediaPlayer;

    public MediaAdapter(String audioType) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer = new VlcPlayer();
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer = new Mp4Player();
        }
    }

    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("vlc")) {
            advancedMediaPlayer.playVlc(fileName);
        } else if (audioType.equalsIgnoreCase("mp4")) {
            advancedMediaPlayer.playMp4(fileName);
        }
    }
}
```

## Step 5: Client (uses only MediaPlayer)

```java
class AudioPlayer implements MediaPlayer {
    private MediaAdapter mediaAdapter;

    public void play(String audioType, String fileName) {
        if (audioType.equalsIgnoreCase("mp3")) {
            System.out.println("Playing mp3 file: " + fileName);
        } else if (audioType.equalsIgnoreCase("vlc") || audioType.equalsIgnoreCase("mp4")) {
            mediaAdapter = new MediaAdapter(audioType);
            mediaAdapter.play(audioType, fileName);
        } else {
            System.out.println("Invalid media type: " + audioType);
        }
    }
}
```

## Step 6: Main

```java
public class AdapterPatternDemo {
    public static void main(String[] args) {
        AudioPlayer player = new AudioPlayer();

        player.play("mp3", "song.mp3");
        player.play("mp4", "video.mp4");
        player.play("vlc", "movie.vlc");
        player.play("avi", "clip.avi");
    }
}
```

## Output:

```
Playing mp3 file: song.mp3
Playing mp4 file: video.mp4
Playing vlc file: movie.vlc
Invalid media type: avi
```

# Benefits

- **Decouples client from adaptee**: You don’t need to modify existing client or adaptee code.
- **Promotes code reuse**: Wraps existing functionality in a new interface.
- **Improves flexibility**: Can adapt to legacy or third-party code easily.

# Drawbacks

- **Added complexity**: More layers of indirection.
- **Increased maintenance**: More classes to understand and maintain.

# Use Cases

- Integrating legacy systems into modern applications.
- Adapting third-party APIs that don’t match your system’s interface.
- Providing a consistent interface over diverse classes.

# Real-World Examples

- **Java IO Streams**: `InputStreamReader` adapts a `byte` stream (`InputStream`) to a `char` stream (`Reader`).
- **Spring’s `AdapterViewController`**: Adapts simple controllers to the `Controller` interface.
- **Wrapper classes in Java Collections**: `Arrays.asList()` adapts an array into a List interface.