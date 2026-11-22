---
tags:
  - OOP
  - "#SECONDARY_BRANCH"
---
Are you really aware of how your TV turns on when you press the ON button on the remote? Do you, as a user, need to know the specific sequence of zero's and one's that your remote control emits to signal to the television’s receiver that it should turn on? Or is pressing the ON button good enough?

Are you really aware of how your car starts when you turn your key in the ignition? Do you, as a driver, need to know about the ignition switch, how the voltage from the battery hits the ignition coil, how an engine spark gets directed to the spark plugs, and how that ignites the fuel to make the car run? Or is turning the key and hearing a ding-ding enough?

![Abstraction makes technology easier to use. Most of us know that pressing the ON button on the remote will turn on a TV. And that's good enough for us. Imagine that you needed to know the low-level electronic details in order to turn your TV on to watch your favourite HBO show. The learning curve would be tremendous. Very few people would watch TV if that were the case.](https://d33wubrfki0l68.cloudfront.net/57d0ec4dbd6a798e516156554448171285453a5f/5903d/img/blog/object-oriented/programming/4-principles/abstraction-oo.png)

Abstraction makes technology easier to use. Most of us know that pressing the ON button on the remote will turn on a TV. And that's good enough for us. Imagine that you needed to know the low-level electronic details in order to turn your TV on to watch your favourite HBO show. The learning curve would be tremendous. Very few people would watch TV if that were the case.

It should be clear that we manoeuvre the world using abstractions — conceptual models that give us a good enough understanding of inventions that are much more complex behind the scenes. And in object-oriented programs, the inventions that we aim to simplify and make easy to use are objects.

Abstraction is a key principle of design. Since object-oriented programs can get quite large, it’s just not humanly possible to expect others to be aware of the inner-workings of each and every class or component within them. Therefore, we strive to abstract objects in a way that makes them intention-revealing, simple to use, and simple to understand.

Consider the abstraction of a washing machine.

```jsx
// Options for the wash cycle
type WashOptions = {
  dryLevel: 'low' | 'medium' | 'high'
  temperature: 'cold',
  duration: 'hour',
  ecoEnabled: false
}

// The abstraction
class WashingMachine {
	
  // Private instance variables
  ... 

  public startCycle (options: WashOptions): void {
	  // Parse the options
    // Get access to the physical layer
    // Convert options into commands
    // Lots of low-level code
    // And so on...
    ...
  }
	
  // More methods
  ...

}
```

Assuming that the only `public` method right now is the `startCycle` method, what else does the client need to know? Does it need to know how `startCycle` works? Does it need to know about the `private` instance variables within the class? Does it need to know about any other `private` methods?

![https://d33wubrfki0l68.cloudfront.net/2be01046dcded55cb57fa42da0c4c5b33681a96b/fb8b2/img/blog/object-oriented/programming/4-principles/washing-machine-abstraction.png](https://d33wubrfki0l68.cloudfront.net/2be01046dcded55cb57fa42da0c4c5b33681a96b/fb8b2/img/blog/object-oriented/programming/4-principles/washing-machine-abstraction.png)

No, absolutely not. Just like the power-button on the TV remote, it’s simplified for public use. For someone to use this abstraction, all they need to know is the existence of the `startCycle` method and how to call it. That’s it. All other details are abstracted away within the class.

```jsx
// Obtain access to the abstraction
import { WashingMachine } from '../washer'
 
// Create an instance
const washer = new WashingMachine();

// Usage
washer.startCycle({
  dryLevel: 'medium',
  temperature: 'cold',
  duration: 'hour',
  ecoEnabled: false
});
```

By keeping what’s not necessary for the client to know abstracted away, we reduce confusion and make it easy for other developers to know how to use our code and extend it.

# Abstraction is the root principle of our design

Abstraction has many uses — for both the designer and the client. There is much more that can be said on abstraction and abstraction tools like interfaces, abstract classes and types, such as the fact that they give us:

- The ability to design using contracts vs concretions
- The ability to separate the what from the how
- The ability to focus on the declarative separately from the imperative.
- The ability to focus on the high-level ideas separately from the low-level details.

But for now, I’ll say that it’s the root principle of design and a central point of focus for many other important topics like [[Design Patterns|design patterns]], principles, responsibility-driven design and more.