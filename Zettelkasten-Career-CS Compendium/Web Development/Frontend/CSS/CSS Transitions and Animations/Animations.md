---
tags:
  - FORMAT
  - CSS
---
Creating animations in CSS allows you to bring your website to life by adding movement and transitions between different states of elements. CSS animations, powered by the **`@keyframes`** rule, allow you to define complex animations with multiple steps. Along with the keyframe rule, several properties help control how the animation behaves, including **`animation-name`**, **`animation-duration`**, **`animation-timing-function`**, and **`animation-delay`**.

# What is the `@keyframes` Rule?

The **`@keyframes`** rule is used to define the states and steps of the animation. It specifies the intermediate steps of the animation, and you can control what the element looks like at various points during the animation.

* **Syntax**:

```
@keyframes animation-name {
  from {
    /* Initial styles */
  }
  to {
    /* Final styles */
  }
}
```

You can also define intermediate steps using percentage values (e.g., `0%`, `50%`, `100%`), which lets you control the animation in more detail.

* **Example**:

```
@keyframes move {
  0% {
    left: 0;
    top: 0;
  }
  100% {
    left: 100px;
    top: 100px;
  }
}
```

This animation, named `move`, moves an element from the top-left corner (`0%` position) to 100px to the right and 100px down (`100%` position).

---

# Key Properties for Animations

Now that we’ve defined the animation using `@keyframes`, we use various CSS properties to control how the animation is applied to an element. These properties are:

# `animation-name`

The **`animation-name`** property specifies which `@keyframes` animation to apply to an element. This ties the animation defined by the `@keyframes` rule to a specific element.

* **Syntax**:

```
animation-name: animation-name;
```

*   
  **Example**:

```
.box {
  animation-name: move; /* Apply the move animation */
}
```

In this example, the `.box` element will use the `move` animation defined in the `@keyframes` rule.

# `animation-duration`

The **`animation-duration`** property specifies how long the animation should take to complete one cycle. It can be set in **seconds** (`s`) or **milliseconds** (`ms`).

* **Syntax**:

```
animation-duration: time;
```

*   
  **Example**:

```
.box {
  animation-duration: 2s; /* The animation will take 2 seconds */
}
```

In this case, the animation will take **2 seconds** to go from the start state to the final state.

# `animation-timing-function`

The **`animation-timing-function`** property defines the speed curve of the animation. It controls how the intermediate states of the animation progress over time. This property works similarly to the `transition-timing-function` property.

* **Common values**:

  * **`linear`**: The animation progresses at a constant speed.

  * **`ease`**: The animation starts slowly, speeds up, and then slows down.

  * **`ease-in`**: The animation starts slowly and speeds up.

  * **`ease-out`**: The animation starts fast and slows down.

  * **`ease-in-out`**: The animation starts and ends slowly but speeds up in the middle.

  * **`cubic-bezier(x1, y1, x2, y2)`**: Custom timing function using a cubic Bézier curve.

* **Syntax**:

```
animation-timing-function: timing-function;
```

*   
  **Example**:

```
.box {
  animation-timing-function: ease-in-out; /* Smooth start and end */
}
```

In this example, the animation will ease in at the start and ease out at the end.

# `animation-delay`

The **`animation-delay`** property specifies how long to wait before starting the animation. The delay can be set in **seconds** (`s`) or **milliseconds** (`ms`).

* **Syntax**:

```
animation-delay: time;
```

*   
  **Example**:

```
.box {
  animation-delay: 1s; /* Wait 1 second before starting the animation */
}
```

In this case, the animation will start **1 second** after the page loads or after the element's state changes.

---

# Shorthand Property for Animations

Instead of specifying each animation property separately, you can use the shorthand **`animation`** property to combine all animation-related properties in a single line.

* **Syntax**:

```
animation: animation-name animation-duration animation-timing-function animation-delay animation-iteration-count;
```

The **`animation-iteration-count`** property can also be used to specify how many times the animation will repeat. It can be set to a number or to **`infinite`** for an infinite loop.

* **Example**:

```
.box {
  animation: move 2s ease-in-out 1s infinite; 
  /* 
    - move: animation name
    - 2s: duration
    - ease-in-out: timing function
    - 1s: delay
    - infinite: repeat infinitely
  */
}
```

This example will apply the **`move`** animation with a **2-second** duration, an **`ease-in-out`** timing function, a **1-second delay**, and the animation will repeat **infinite** times.

---

# Example of a Complete CSS Animation

Let’s create a simple animation that moves a box from left to right:

```
@keyframes moveRight {
  0% {
    transform: translateX(0);
  }
  100% {
    transform: translateX(300px);
  }
}

.box {
  width: 100px;
  height: 100px;
  background-color: blue;
  animation-name: moveRight; /* Apply the moveRight animation */
  animation-duration: 2s; /* Duration is 2 seconds */
  animation-timing-function: ease-in-out; /* Smooth transition */
  animation-delay: 1s; /* Wait 1 second before starting the animation */
}
```

# Explanation of the Example:

1. **`@keyframes moveRight`**: Defines the animation named `moveRight`. The box starts at `transform: translateX(0)` (position 0), and at `100%`, it moves to `transform: translateX(300px)` (position 300px).

2. **`.box`**: This class applies the animation to the element.

   * **`animation-name: moveRight`**: Links the `moveRight` keyframe animation to the `.box` element.

   * **`animation-duration: 2s`**: Specifies that the animation should take **2 seconds** to complete.

   * **`animation-timing-function: ease-in-out`**: The animation will start and end slowly.

   * **`animation-delay: 1s`**: The animation will wait **1 second** before starting.

# Animation with Repeats

If you want the animation to repeat a certain number of times, you can use the **`animation-iteration-count`** property:

```
.box {
  animation: moveRight 2s ease-in-out 1s 5; /* Repeat 5 times */
}
```

For infinite repeats, use **`infinite`**:

```
.box {
  animation: moveRight 2s ease-in-out 1s infinite; /* Repeat infinitely */
}
```

---

# Conclusion

CSS animations provide a powerful way to create complex animations directly in your stylesheets. By using **`@keyframes`** to define the stages of the animation and properties like **`animation-name`**, **`animation-duration`**, **`animation-timing-function`**, and **`animation-delay`**, you can control how the animation behaves. The shorthand **`animation`** property allows you to define all aspects of the animation in a single line, making it easier to manage. With these tools, you can create engaging and dynamic effects for your web pages.

