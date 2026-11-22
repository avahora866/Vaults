---
tags:
  - FORMAT
  - CSS
---
Absolutely\! Let’s break down the **key Flexbox properties** and what each one does. These properties are essential for building flexible and responsive layouts in CSS using Flexbox.

---

# `display: flex`

This is the **starting point** for using Flexbox. It makes the element a **flex container**, and its direct children become **flex items**.

```
.container {
  display: flex;
}
```

---

# `flex-direction`

Defines the **main axis**—the direction in which flex items are laid out.

* `row` (default): left to right

* `row-reverse`: right to left

* `column`: top to bottom

* `column-reverse`: bottom to top

```
.container {
  flex-direction: row; /* or column, row-reverse, column-reverse */
}
```

---

# `justify-content`

Aligns flex items **along the main axis** (horizontal in `row`, vertical in `column`).

* `flex-start`: items start at the beginning

* `flex-end`: items at the end

* `center`: items in the center

* `space-between`: equal space between items

* `space-around`: equal space around items

* `space-evenly`: equal space between and around

```
.container {
  justify-content: space-between;
}
```

---

# `align-items`

Aligns items **along the cross axis** (vertical if `flex-direction` is `row`).

* `stretch` (default): items stretch to fill

* `flex-start`: items align to top

* `flex-end`: items align to bottom

* `center`: items vertically centered

* `baseline`: aligns items based on text baseline

```
.container {
  align-items: center;
}
```

---

# `align-self`

Used on **individual flex items** to override `align-items` from the container.

```
.item {
  align-self: flex-end;
}
```

---

# `flex-wrap`

By default, flex items try to fit on one line. This property controls whether they wrap onto multiple lines.

* `nowrap` (default): no wrapping

* `wrap`: items wrap to the next line

* `wrap-reverse`: wrap in reverse order

```
.container {
  flex-wrap: wrap;
}
```

---

# `flex-grow`

Defines how much a flex item **can grow** relative to others. A value of `1` means it can grow and share space with other items that also have `flex-grow`.

```
.item {
  flex-grow: 1;
}
```

---

# `flex-shrink`

Defines how much a flex item **can shrink** when there’s not enough space. A value of `0` means it won't shrink, `1` means it will.

```
.item {
  flex-shrink: 1;
}
```

---

# `flex-basis`

Defines the **initial size** of a flex item before any growing or shrinking.

```
.item {
  flex-basis: 200px;
}
```

You can combine `flex-grow`, `flex-shrink`, and `flex-basis` using the shorthand:

```
.item {
  flex: 1 1 200px;
}
/* flex: grow shrink basis */
```

---

Let me know if you'd like a quick visual layout example using these properties, or a small project idea to practice with.

