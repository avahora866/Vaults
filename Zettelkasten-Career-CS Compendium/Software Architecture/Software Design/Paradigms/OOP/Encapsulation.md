---
tags:
  - OOP
  - "#SECONDARY_BRANCH"
---
Objects contain state, make decisions based on that state, and relay messages to other objects asking them for help to fetch or compute things, like a network of routers or a web of atoms.

Because objects make decisions (and enact business logic) based on their state, it’s important that each object manage its own state and protect against mutations from outside classes. Failure to do so can lead to strange behaviour, bugs, and an [anemic domain model](https://khalilstemmler.com/wiki/anemic-domain-model/).

Encapsulation is the technique of making state `private`. Encapsulation means that the class which owns the state decides the degree to which state can be accessed and changed. This is done through the use of `public` methods.

Let’s continue to demonstrate with the washing machine example. Depending on the washing machine’s current state, some behaviours are valid while others are invalid.

![https://d33wubrfki0l68.cloudfront.net/b7a4a0d22f79390098736a2a7ad4cb8753d98135/8c328/img/blog/object-oriented/programming/4-principles/washing-machine.png](https://d33wubrfki0l68.cloudfront.net/b7a4a0d22f79390098736a2a7ad4cb8753d98135/8c328/img/blog/object-oriented/programming/4-principles/washing-machine.png)

For example, if the machine is currently `ON` , it is valid to call `turnOff()` to turn the machine `OFF`. If the machine is in the `WASH` state, it’s OK to call `pause()` to put the machine into the `PAUSED` state. But if the machine is `OFF` (or `ON` for that matter) it would not be valid to call `PAUSE`.

Why? Because a washing machine can’t go from being `OFF` to `PAUSED`. It doesn’t make sense.

Encapsulation is the mechanism that governs these rules (also known as [class invariants](https://khalilstemmler.com/wiki/invariant/)) for us.

```jsx
type WashState = 'OFF' | 'ON' | 'WASH' | 'PAUSED' | 'DONE';
type Cycle = undefined | 'COOL' | 'WARM';

type MachineState = {
  washState: WashState;
  currentCycle: Cycle;
}

class WashingMachine {
  // Encapsulation of state is achieved here by making
  // state private and inaccessible directly.
  private state: MachineState;
	
  // Access is provided via public methods
  public getWashState () : WashState {
    return this.state.washState;
  }
  
  public getCurrentCycle (): Cycle {
    return this.state.currentCycle;
  }

  // State is changed only through the use of public
  // mutator methods
  public pause (): void {
    // Note this important business logic which is encapsulated
    // to the correct place. The related data and behavior live
    // together.

    // We ONLY do the pause logic if the current state is
    // 'WASH'
    if (this.state.washState === 'WASH') {
      // Stop washing
      ...
      // Cue pause sound
      ...
      // Set new state
      this.state.washState = 'PAUSED'
    }
  }

	...
}
```

Given this, client usage may look as follows:

```jsx
washer.startCycle({
  dryLevel: 'medium',
  temperature: 'cold',
  duration: 'hour',
  ecoEnabled: false
});

console.log(washer.getWashState()); // 'WASH'
washer.pause();
console.log(washer.getWashState()); // 'PAUSED'
```

Encapsulation combines together the data and related behavior which — like [[Software Architecture/Software Design/Paradigms/OOP/Abstraction|abstraction]] — simplifies usage, but also acts as a single place for us to put relevant logic. With this technique, we invent single sources of truth and obtain appropriate places to locate logic once and for all.
