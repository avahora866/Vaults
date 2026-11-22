---
tags:
  - "#TRUNK"
---
# The Conceptual Foundation

## Component-Based Architecture
In traditional web development (plain HTML/JS), we often think of a webpage as a single, large document. React changes this paradigm. It asks you to view your User Interface (UI) not as a page, but as a collection of isolated, reusable building blocks called Components.

Imagine a standard Twitter (X) feed. In React, you would not write one giant HTML file. Instead, you would build:
- A Navbar component.
- A ProfileCard component.
- A Tweet component.
- A Feed component (which renders a list of Tweet components).

Why is this superior?
- Reusability: Once you build a Button component, you can use it all over your app.
- Isolation: If the Tweet component breaks, it likely won't crash the entire Navbar.

## The Virtual DOM and Reconciliation
This is the most critical concept for understanding React's performance.

The "Real DOM" (Document Object Model) is the browser's structure of your webpage. It is an actual tree of objects that the browser renders.
- The Problem: Manipulating the Real DOM is computationally expensive. Every time you change an element (like updating a notification counter from 1 to 2), the browser often has to re-calculate the layout and re-paint the screen. If you do this thousands of times rapidly, the UI becomes sluggish.
- The Solution (The Virtual DOM): React keeps a lightweight copy of the Real DOM in memory, known as the Virtual DOM.

### The Process (Reconciliation):
1. Render: When data changes in your app, React updates the Virtual DOM first. This is incredibly fast because it is just JavaScript objects, not the actual screen.
2. Diffing: React compares the new Virtual DOM with the previous version of the Virtual DOM. It calculates exactly what changed.
3. Commit: React updates the Real DOM, but only changes the specific parts that were altered.

Analogy: Imagine you are an architect renovating a house.
- Direct DOM Manipulation: You walk into the house and knock down a wall every time you have a new idea. This is slow, dusty, and expensive.
- React (Virtual DOM): You draw the changes on a blueprint (The Virtual DOM). You erase a wall on paper and redraw it. This is instant. Once the blueprint is perfect, you give the construction crew instructions to change only that specific wall in the real house.

# Syntax and Structure (JSX)
If you look at a React file, it often looks confusing to new students because it appears we are writing HTML directly inside JavaScript logic. This syntax is called JSX (JavaScript XML).

## What is JSX?
JSX is a syntax extension for JavaScript. It produces React "elements." While it looks like HTML, it is not HTML. Browsers cannot read JSX directly. Under the hood, a tool (usually Babel) converts JSX into standard JavaScript objects that React can understand.

Example:
What you write (JSX):
```js
const element = <h1 className="greeting">Hello, World!</h1>;
```
What the browser actually receives (Standard JS):
```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, World!'
);
```
Note: You do not need to memorize the bottom code, but you must understand that JSX is syntactic sugar.

## The Three Golden Rules of JSX
To write valid JSX, you must adhere to strict rules that differ slightly from HTML.

Rule A: The Single Parent Rule In HTML, you can write two siblings side-by-side. In JSX, a component must return a single parent element.
Wrong:
```js
return (
  <h1>Title</h1>
  <p>Paragraph</p>
);
```
Why? A JavaScript function cannot return two separate values at once.
Correct:
```js
return (
  <div>
    <h1>Title</h1>
    <p>Paragraph</p>
  </div>
);
```
Alternative: If you don't want to add an extra <div> to the DOM, you can use a Fragment: <> ... </>.

Rule B: CamelCase Attributes Since JSX is closer to JavaScript than HTML, we use camelCase for attributes.
- HTML: onclick, tabindex
- JSX: onClick, tabIndex

Rule C: The className Distinction This is the most common error for beginners. class is a reserved keyword in JavaScript (used for defining classes). Therefore, we cannot use it for CSS classes.
- HTML: <div class="sidebar">
- JSX: <div className="sidebar">

# The Building Blocks (Components & Props)
## Functional Components
In modern React, a component is simply a JavaScript function that returns JSX.

There is one strict rule you must memorize immediately: Component names must start with a Capital Letter.
- function myComponent() -> React treats this as a regular HTML tag.
- function MyComponent() -> React treats this as a custom component.

Example of a simple component:
```js
function WelcomeMessage() {
  return <h1>Welcome to the Class!</h1>;
}
```

## Dynamic Data with Curly Braces {}
To unleash the power of JavaScript inside your JSX (the "J" in JSX), we use curly braces. Anything inside {} is evaluated as JavaScript code.
```js
function CurrentDate() {
  const date = "December 3rd";
  // The braces allow the variable 'date' to be read
  return <p>Today is {date}</p>; 
}
```
## Props (Properties)
Components would be useless if they always displayed the same static text. We need them to be dynamic. We achieve this using Props.

Think of Props exactly like arguments in a function.
- When you call a regular function: add(2, 3)
- When you use a Component: <element attribute="value" />

The data flows down from the Parent Component to the Child Component.

Step A: The Parent passes the data
```js
function App() {
  return (
    <div>
      {/* We pass a prop called 'name' with the value 'Alice' */}
      <Greeting name="Alice" />
      <Greeting name="Bob" />
    </div>
  );
}
```
Step B: The Child receives the data React automatically bundles all attributes passed to the component into a single object called props.
```js
// The function receives the 'props' object
function Greeting(props) {
  // We access the data via props.name
  return <h1>Hello, {props.name}!</h1>;
}
```
Important Constraint: Props are Read-Only (Immutable). A child component cannot change its own props. If a parent passes name="Alice", the child cannot say props.name = "Steve". It must respect the data given to it.

# Interactivity and State (The useState Hook)
We now cross the threshold from static "posters" to dynamic "applications."

Up to this point, our components have been static. If we wanted to change the text on the screen, we had to change the code or the props passed down from a parent.

To change data inside a component while the app is running (e.g., typing in a form, clicking a counter), we need State.

## State vs. Props
- Props: Passed in from outside (like function arguments). They are immutable (cannot be changed by the component).
- State: Managed inside the component (like local variables). It is mutable (changeable), but only via a specific method.

## The useState Hook
To use state, we import a "Hook" called useState from React. A Hook is just a special function that lets you "hook into" React features.

The Syntax:
```js
import { useState } from 'react';

function Counter() {
  // syntax: const [currentValue, functionToUpdateValue] = useState(initialValue);
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      {/* We use the updater function on click */}
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}
```
1. useState(0): We tell React we want a piece of state, initialized at 0.
2. count: The variable that holds the current value.
3. setCount: The only way to change that variable.
4. [...]: This is standard JavaScript array destructuring. We are just naming the two things React gives us.

## The Golden Rule of State
Never modify state directly.
- Wrong: count = 5;
  - Why? React won't know the data changed, so it won't re-render the Virtual DOM. The screen will stay the same.
- Correct: setCount(5);
  - Why? This triggers React to re-run the component function, generate a new Virtual DOM, and update the screen.

## State variables being passed as props (Unidirectional Data Flow (One-Way Data Flow))
Inside the Child, it is a Prop.
The Transformation
1. In the Parent (The Owner): The data is State. The parent "owns" this memory. It can change it using setState.
2. In the Child (The Receiver): The moment that data crosses the boundary into the Child component, it becomes a Prop. The Child does not know (and does not care) that the data was "state" in the parent. It just sees a value arriving.
  - The Child cannot change it.
  - The Child can only read it.

The Analogy: The Landlord and the Tenant
- The Parent (Landlord): Owns the thermostat (State). The Landlord can turn the dial to change the temperature.
- The Child (Tenant): Lives in the room. They feel the temperature (Prop).
- The Reality: The Tenant cannot turn the dial. They just experience whatever temperature the Landlord sets. If the Landlord changes the State (turns dial), the Tenant's Prop (ambient temp) updates automatically.

```js
// 1. PARENT COMPONENT
function Parent() {
  // Here, 'money' is STATE. The Parent can change it.
  const [money, setMoney] = useState(100);

  return (
    <div>
      <h1>Parent has: ${money}</h1>
      {/* We pass the State down. It enters the Child as a Prop. */}
      <Child cash={money} /> 
    </div>
  );
}

// 2. CHILD COMPONENT
function Child(props) {
  // Here, 'props.cash' is just a READ-ONLY value.
  // The child doesn't know it came from a useState hook.
  
  // ❌ ILLEGAL: props.cash = 500; (Cannot assign to read only property)
  
  return <h2>Child received: ${props.cash}</h2>;
}
```
### The "Magic" of Re-rendering
This is where the power of React lies: If the Parent calls setMoney(200), React detects a state change. It re-renders the Parent. Because the Parent includes the Child, React re-renders the Child too, passing the new number (200) as the prop.

To the Child, it looks like the prop just magically updated.

## How can a child modify a state passed in
Because JavaScript functions are "first-class citizens" (meaning they are treated just like variables), you can pass the setMoney function down to the child as a prop

This creates a beautiful loop:
1. Data flows Down (Parent $\rightarrow$ Child).
2. Events/Function Calls flow Up (Child $\rightarrow$ Parent).

```js
// 1. PARENT
function Parent() {
  const [money, setMoney] = useState(100);

  return (
    <div>
      <h1>Wallet: ${money}</h1>
      {/* We pass the VALUE of money (state -> prop)
         AND we pass the FUNCTION to change it (function -> prop)
      */}
      <Child 
        cash={money} 
        giveMoneyBack={setMoney} 
      />
    </div>
  );
}

// 2. CHILD
function Child(props) {
  return (
    <div>
      <p>I have access to: ${props.cash}</p>
      
      {/* When clicked, we call the function the Parent gave us */}
      <button onClick={() => props.giveMoneyBack(props.cash - 10)}>
        Spend $10
      </button>
    </div>
  );
}
```
1. When the user clicks the button in the Child:
2. It executes props.giveMoneyBack(90).
3. props.giveMoneyBack is actually a reference to the Parent's setMoney function.
4. The Parent's state updates to 90.
5. React sees the Parent changed, so it re-renders the Parent.
6. React re-renders the Child with the new cash prop of 90.

# Side Effects and Lifecycle (The useEffect Hook)
Up until now, our components have been "pure." They simply take data (Props/State) and output UI (JSX). They calculated nothing outside of themselves.

But real applications interact with the outside world. They need to:
- Fetch data from a server (API calls).
- Manually change the DOM (e.g., changing the page title).
- Set up subscriptions or timers.

In React, these operations are called Side Effects. We handle them using the useEffect hook.

## The Mental Model
Think of useEffect as a function that runs after React renders the component to the screen.
1. React updates the DOM (paints the screen).
2. React runs your "Effect" function.

## The Syntax
```js
import { useEffect } from 'react';

useEffect(() => {
  // Your code here (The Effect)
}, [ dependencies ]); // The Dependency Array
```
There are two parts:
1. The Effect Function: The code you want to run.
2. The Dependency Array: This controls when the code runs.

## The Dependency Array (Crucial)
This array is the "trigger" for your effect. Understanding this is the difference between a bug-free app and an infinite loop.

There are three scenarios:

### Scenario A: The Empty Array []
- Behavior: Run this code ONLY ONCE, immediately after the component first appears (Mounts).
- Use Case: Fetching initial data from an API or setting up a global event listener.
```js
useEffect(() => {
  console.log("I run only when the component first loads.");
}, []);
```

### Scenario B: The Array with Variables [count, userId]
- Behavior: Run this code when the component first appears, AND whenever count or userId changes value.
- Use Case: Fetching new user data when the userId changes.
```js
useEffect(() => {
  console.log("I run on load, and whenever 'count' changes.");
}, [count]);
```
### Scenario C: No Array (Omitted)
- Behavior: Run this code EVERY SINGLE TIME the component renders.
- Use Case: extremely rare. Avoid this unless you have a very specific reason.
```js
useEffect(() => {
  console.log("I run constantly. This might be dangerous.");
});
```

### A Practical Example
Let's create a component that updates the browser tab's title (the document title) whenever you click a button.
```js
import { useState, useEffect } from 'react';

function TitleUpdater() {
  const [count, setCount] = useState(0);

  // This effect runs every time 'count' changes
  useEffect(() => {
    document.title = `You clicked ${count} times`;
  }, [count]); // <--- The dependency

  return (
    <button onClick={() => setCount(count + 1)}>
      Click me
    </button>
  );
}
```
If we did not include [count] in the array, or if we left it empty [], the title would set once on load (to "0 times") and never update again, even if the button count increased. The array tells React: "Keep an eye on count. If it changes, run this effect again."

### Reason for useEffect
#### The "Infinite Loop" Trap
React components are just functions. They run (render) whenever anything changes (props or state).

Imagine you put an API call directly inside the component body, without useEffect:
```js
// ❌ DANGEROUS CODE
function BadComponent() {
  const [data, setData] = useState(null);

  // This code runs EVERY time the function runs
  fetch('/api/user').then(user => {
    // This updates state...
    setData(user); 
    // ...which triggers a re-render...
    // ...which runs the function again...
    // ...which fetches again...
    // ...which sets state again...
  });

  return <div>{data}</div>;
}
```
Result: This creates an infinite loop. Your browser will crash. The Fix: useEffect moves this logic out of the rendering loop. It tells React: "Wait until the screen is painted, then check if you need to do this."

#### Synchronization (Declarative vs. Imperative)
Imperative ( The Old Way / Without useEffect): If you have a "Search" input, you have to manually wire up the API call to the typing event.
- "When user types -> Update State -> AND Call API."
- "When user clicks Clear -> Update State -> AND Call API."
- "When user clicks Undo -> Update State -> AND Call API."

If you forget to add the "Call API" line to any of those buttons, your data is out of sync.

Declarative (The React Way / With useEffect): You simply tell React: "Whenever the query variable changes, fetch new data."
```js
// ✅ The React Way
  useEffect(() => {
    fetchResults(query);
  }, [query]); // <--- The dependency
```
Now, it doesn't matter how query changed (typing, clearing, undoing, voice command). As long as the state changes, the API call happens automatically. You don't have to "remember" to call it in every single button handler.

---
In Summary:
- Safety: It keeps side effects (API calls) out of the main rendering process so the UI shows up instantly.
- Automation: It guarantees that your "Outside World" (API data) is always synchronized with your "Internal World" (State).

# Context Management (The useContext Hook)
To understand Context, we must first understand the pain it cures: Prop Drilling.

## The Problem: Prop Drilling
Imagine you are building an app with this hierarchy: App -> Dashboard -> Settings -> Profile -> Avatar

If App holds the user's profile picture, but only Avatar needs to display it, standard React rules (Unidirectional Data Flow) force you to pass that prop through every single layer:
1. App passes pic to Dashboard (who ignores it).
2. Dashboard passes pic to Settings (who ignores it).
3. Settings passes pic to Profile (who ignores it).
4. Profile passes pic to Avatar (who finally uses it).
5. This is Prop Drilling. It makes your code messy, brittle, and annoying to maintain.

## The Solution: Context
Context provides a way to pass data through the component tree without having to pass props down manually at every level. It works like a Teleporter or a Radio Broadcast.
- Prop Drilling: Passing a bucket of water hand-to-hand down a line of 50 people.
- Context: Turning on a sprinkler system that reaches everyone in the yard instantly.

## The Implementation (3 Steps)
Implementing Context requires three distinct parts: Create, Provide, and Consume.

Step A: Create the Context First, we create the "Radio Station." This is usually done in a separate file or at the top of your component file.
```js
import { createContext } from 'react';

// Create the context. We can give it a default value (optional).
const ThemeContext = createContext(null);
```
Step B: Provide the Context (The Broadcast) We wrap the part of our app that needs the data in a Provider. Any component inside this wrapper can access the data.
```js
import { useState } from 'react';

function App() {
  const [theme, setTheme] = useState("dark");

  return (
    // We broadcast the 'theme' value to everyone inside
    <ThemeContext.Provider value={theme}>
      <Dashboard /> 
    </ThemeContext.Provider>
  );
}
```
Note: Even though Dashboard is the direct child, components DEEP inside Dashboard will still receive the signal.

Step C: Consume the Context (The Receiver) Now, any component deep in the tree (like Button) can access the data directly, bypassing its parents.
```js
import { useContext } from 'react';

function Button() {
  // We tune into the station using the useContext hook
  const theme = useContext(ThemeContext);

  return <button className={theme}>I am a {theme} button</button>;
}
```

# Complex State & Reducers (The useReducer Hook)
You are already comfortable with useState. It is perfect for simple things: a counter, a form field, a toggle.

However, when your state becomes complex—for example, an object with 5 properties that depend on each other, or a list where adding an item also needs to update a total count—useState can get messy. You end up with five different setState calls scattered everywhere.

Enter useReducer.

## The Analogy: The Bank Teller
- useState (The Wallet): You open your wallet and take money out directly. It's fast, but if you're drunk or careless, you might lose track.
- useReducer (The Bank Teller): You cannot touch the money in the vault directly. Instead, you slip a piece of paper (an Action) to the teller. The paper says "DEPOSIT $50." The teller (the Reducer Function) reads the paper, follows strict rules, and updates the vault for you.

## The Three Parts
1. The State: The data (e.g., { count: 0 }).
2. The Action: An object describing what happened (e.g., { type: 'INCREMENT' }).
3. The Reducer Function: A pure function that takes the current state and an action, and returns the new state. 

## The Syntax
Step A: Define the Reducer Function This acts as the rulebook. It usually lives outside your component because it doesn't need to know about the component rendering
```js
function bankReducer(state, action) {
  // We check the "type" of action to decide what to do
  switch (action.type) {
    case 'DEPOSIT':
      return { balance: state.balance + action.amount };
    case 'WITHDRAW':
      return { balance: state.balance - action.amount };
    default:
      return state; // If we don't recognize the action, do nothing
  }
}
```
Step B: Use the Hook
```js
import { useReducer } from 'react';

function BankAccount() {
  // Syntax: [state, dispatch] = useReducer(reducerFunction, initialState)
  const [state, dispatch] = useReducer(bankReducer, { balance: 0 });

  return (
    <div>
      <h1>Balance: ${state.balance}</h1>
      {/* We "dispatch" an action instead of setting state directly */}
      <button onClick={() => dispatch({ type: 'DEPOSIT', amount: 100 })}>
        Deposit $100
      </button>
      <button onClick={() => dispatch({ type: 'WITHDRAW', amount: 50 })}>
        Withdraw $50
      </button>
    </div>
  );
}
```
Why is this better for complex apps?
- Centralized Logic: All the logic for modifying state is in one function (bankReducer), not scattered across 10 different button handlers.
- Predictability: You can easily see every possible way the state can change just by reading the switch statement.

# References & DOM Access (The useRef Hook)
In React, we usually let the Virtual DOM handle everything. We rarely touch the actual HTML elements directly. However, sometimes we must. For example:
- Focusing an input field automatically when a popup opens.
- Scrolling to a specific part of the page.
- Measuring the width of an element.

For this, we use useRef.

## The "Box" Analogy
Think of useRef as a storage box.
- You can put something in it.
- You can take something out.
- Crucial Difference: When you put something in or take it out, React does not notice. It does not trigger a re-render.

## Syntax and Usage
```js
import { useRef } from 'react';

function TextInput() {
  // 1. Create the ref. It starts as null.
  const inputRef = useRef(null);

  const focusInput = () => {
    // 3. Access the DOM node using .current
    // standard DOM API: element.focus()
    inputRef.current.focus();
  };

  return (
    <div>
      {/* 2. Attach the ref to an element */}
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus the Input</button>
    </div>
  );
}
```
Key Property: ref.current The useRef hook returns an object that looks like this: { current: ... }. The value is always stored inside .current.

## The "Silent" Variable (Advanced Use Case)
You can also use useRef to store data that is NOT a DOM element. If you have a value that changes often (like a timer ID or a coordinate tracking mouse position) but you don't want the screen to re-paint every millisecond, use a ref.
- useState: "Change me -> Re-render the UI."
- useRef: "Change me -> Keep it secret, keep it safe. No re-renders."

# Performance Optimization (useMemo and useCallback)
React is fast by default, but in large applications, "wasted renders" can slow things down.

The Problem: Every time a component re-renders, every line of code inside it runs again.
1. Complex math calculations are recalculated.
2. Functions are re-created from scratch (receiving new memory addresses).

We use useMemo and useCallback to "cache" (memoize) these things so we don't do unnecessary work.

## useMemo (Caching a Value)
Imagine you have a function slowSquare(n) that takes 2 seconds to run. If you render the component 10 times, you don't want to wait 2 seconds every time if n hasn't changed.

useMemo tells React: "Remember this result. Only recalculate it if the input changes."
```js
import { useMemo, useState } from 'react';

function Calculator({ number }) {
  // ❌ Without useMemo: Runs on every single render, slowing the app
  // const squared = slowSquare(number);

  // ✅ With useMemo: Runs ONLY when 'number' changes
  const squared = useMemo(() => {
    return slowSquare(number);
  }, [number]); // Dependency array works just like useEffect

  return <div>Result: {squared}</div>;
}
```

## useCallback (Caching a Function)
This is slightly more abstract.

In JavaScript, functions are objects. function() {} is not equal to function() {}. Every time a component re-renders, it creates a brand new version of every function inside it.

If you pass a function to a child component as a prop, the child says: "Oh, a new function! I must re-render."

useCallback freezes the function so it stays the same between renders.
```js
import { useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);

  // This function is "stable". It won't be re-created when 'count' changes.
  const handleClick = useCallback(() => {
    console.log('Button clicked');
  }, []); 

  return <Child onClick={handleClick} />;
}
```

## The Rule of Thumb:
- Use useMemo when you want to cache a calculated value (like a filtered list or a math result).
- Use useCallback when you want to cache a function definition (usually to pass to a child component).

# Custom Hooks (Abstracting Logic)
You now know the primitives: useState, useEffect, useContext, etc. However, professional React development is about composition. You will often find yourself repeating the same logic (e.g., fetching data, listening to window size, connecting to a socket) in multiple components.

Instead of copy-pasting code, we extract that logic into a Custom Hook.

## What is a Custom Hook?
It is simply a JavaScript function that:
1. Starts with the word use (e.g., useFetch, useWindowSize).
2. Calls other hooks inside it.

## Example: The useFetch Hook
Imagine you have 5 different components that all need to load data from an API. Instead of writing useEffect and fetch 5 times, we write it once.

The Hook Definition:
```js
// useFetch.js
import { useState, useEffect } from 'react';

function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    setLoading(true);
    fetch(url)
      .then(response => response.json())
      .then(json => {
        setData(json);
        setLoading(false);
      });
  }, [url]); // Re-run if the URL changes

  // Return the data the component needs
  return { data, loading };
}
```
The Component Usage:
```js
function UserProfile() {
  // The component is now incredibly clean
  const { data, loading } = useFetch('https://api.my-app.com/user/1');

  if (loading) return <p>Loading...</p>;
  return <h1>{data.name}</h1>;
}
```
Why this is powerful:
- Reusability: You can use useFetch in 100 different components.
- Separation of Concerns: The component focuses on UI, the hook focuses on Data.