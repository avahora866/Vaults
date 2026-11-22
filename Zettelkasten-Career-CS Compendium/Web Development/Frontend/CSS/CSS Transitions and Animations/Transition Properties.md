---
tags:
  - FORMAT
  - CSS
---
In CSS, the **transition** property is used to create smooth changes between different states of an element, and there are several key properties you can control to define how the transition behaves. Let's break down each of the four key properties: **`transition-property`**, **`transition-duration`**, **`transition-timing-function`**, and **`transition-delay`**.

# `transition-property`

The `transition-property` property specifies which CSS properties should be transitioned (i.e., animated) when their values change.

* **Syntax**:

```
transition-property: property;
```

*   
  Where `property` can be a single CSS property (e.g., `background-color`, `opacity`, `width`), a comma-separated list of properties, or the special value `all` to apply the transition to all properties that change.

* **Example**:

```
div {
  transition-property: background-color, transform;
}
```

*   
  In this example, only the `background-color` and `transform` properties will animate during the transition. If you use `all`, it applies to all animatable properties that change during the transition.

```
div {
  transition-property: all;
}
```

*   
  This would cause any property that changes on the `div` to be animated.

---

# `transition-duration`

The `transition-duration` property defines how long the transition should take. The value is typically specified in seconds (`s`) or milliseconds (`ms`).

* **Syntax**:

```
transition-duration: time;
```

*   
  Where `time` is the duration for the transition, e.g., `0.5s` for half a second, `2s` for two seconds, or `300ms` for 300 milliseconds.

* **Example**:

```
div {
  transition-duration: 0.5s;
}
```

*   
  This means the transition will last for **half a second**.

---

# `transition-timing-function`

The `transition-timing-function` property defines how the intermediate states of the transition will be paced over time. It controls the speed curve of the transition, determining whether the transition progresses at a constant pace or with some acceleration or deceleration.

* **Syntax**:

```
transition-timing-function: function;
```

*   
  The `function` can be one of the following:

  * **`linear`**: The transition progresses at a constant speed (no acceleration or deceleration).

  * **`ease`**: The transition starts slowly, accelerates in the middle, and slows down again (this is the default value).

  * **`ease-in`**: The transition starts slowly and then speeds up.

  * **`ease-out`**: The transition starts fast and then slows down.

  * **`ease-in-out`**: The transition starts and ends slowly but speeds up in the middle.

  * **`cubic-bezier(x1, y1, x2, y2)`**: You can define a custom timing function using a cubic Bézier curve, giving you fine-grained control over the transition speed.

* **Example**:

```
div {
  transition-timing-function: ease-in;
}
```

*   
  This means the transition will start slowly and then speed up as it progresses.

---

# `transition-delay`

The `transition-delay` property specifies when the transition should start. By default, the transition begins immediately after a state change, but you can delay its start by specifying a time in seconds or milliseconds.

* **Syntax**:

```
transition-delay: time;
```

*   
  Where `time` is the delay before the transition starts, e.g., `1s` (one second) or `500ms` (500 milliseconds).

* **Example**:

```
div {
  transition-delay: 0.5s;
}
```

*   
  In this case, the transition will begin **half a second** after the state change.

---

# Putting It All Together

You can combine all four of these properties in a shorthand version using the `transition` property, which simplifies the declaration:

```
transition: [transition-property] [transition-duration] [transition-timing-function] [transition-delay];
```

For example:

```
div {
  transition: background-color 0.5s ease-in 0.2s;
}
```

This is equivalent to:

```
div {
  transition-property: background-color;
  transition-duration: 0.5s;
  transition-timing-function: ease-in;
  transition-delay: 0.2s;
}
```

# Example Scenario

Let's apply all these properties in a practical example. Suppose you want a box to change its background color and scale smoothly when hovered over, with a delayed and easing transition.

```
div {
  width: 200px;
  height: 200px;
  background-color: blue;
  transition: background-color 0.3s ease-in-out, transform 0.5s ease-out 0.2s;
}

div:hover {
  background-color: red;
  transform: scale(1.2);
}
```

## Breakdown:

* **`background-color`**: Changes over **0.3 seconds** with an **ease-in-out** timing function.

* **`transform`**: Scales the element by **1.2x** over **0.5 seconds**, starting **0.2 seconds** after the hover event begins (due to the `transition-delay`).

---

# Conclusion

The four key properties of CSS transitions—**`transition-property`**, **`transition-duration`**, **`transition-timing-function`**, and **`transition-delay`**—allow you to control how an element's style changes over time when a state change occurs. By understanding and using these properties, you can create smooth, interactive animations on your website that improve the user experience.

