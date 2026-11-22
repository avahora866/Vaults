---
tags:
  - Trigonometry
  - FORMAT
---
The Law of Sines and Law of Cosines are fundamental trigonometric identities used for solving non-right triangles.

## Law of Sines

The Law of Sines states that for any triangle ABC with sides a, b, c opposite to angles A, B, C respectively:

$$
\frac{a}{\sin A} = \frac{b}{\sin B} = \frac{c}{\sin C}
$$


### Derivation of the Law of Sines

Consider a triangle . We draw an altitude from vertex to side , forming two right triangles.

Using basic trigonometry:
$$
h = b \sin A
$$

$$
h = a \sin B
$$
Since both expressions equal , we equate them:
$$
b \sin A = a \sin B
$$

Rearrange:
$$
\frac{a}{\sin A} = \frac{b}{\sin B}
$$


Repeating this process for the other angles gives the complete Law of Sines.

### When to Use the Law of Sines

- Given ASA (Angle-Side-Angle) or AAS (Angle-Angle-Side) information.
- Finding unknown angles or sides in non-right triangles.

### Example

Given the following triangle, where:

- Angle A  = 30°
- Angle B = 45°
- Side  a = 10 (opposite angle A)


We want to find side b (opposite angle B).

We will use the Law of Sines, which is:

$$
\frac{a}{\sin A} = \frac{b}{\sin B}
$$

1. Set up the equation:

We know a = 10, A = 30° and B = 45° so:

$$
\frac{10}{\sin 30^\circ} = \frac{b}{\sin 45^\circ}
$$
2. Substitute the values of sine for the angles:
	- Sin 30° = 0.5
	- Sin 45° ≈ 0.707
So the equation becomes:

$$
$$
$$
\frac{10}{0.5} = \frac{b}{0.707}
$$
$$
$$

3. Solve for b:
$$
20 = \frac{b}{0.707}
$$

Multiply both sides by :

$$
b = 20 \times 0.707 \approx 14.14
$$

Conclusion:

The length of side  is approximately 14.14.

This is how we used the Law of Sines to solve for an unknown side in a triangle.

---

## Law of Cosines

The Law of Cosines relates the sides of a triangle to the cosine of one of its angles:

$$
c^2 = a^2 + b^2 - 2ab\cos C
$$

Similarly, we can write:

$$
a^2 = b^2 + c^2 - 2bc\cos A
$$


$$
b^2 = a^2 + c^2 - 2ac\cos B
$$


### Derivation of the Law of Cosines

Consider , where we set up a coordinate system with:

From the distance formula, the length of side a is:

$$
a^2 = (x - c)^2 + y^2
$$
Expressing and in terms of and :



$$
x = b \cos C, \quad y = b \sin 
$$
Substituting:

$$
a^2 = (b \cos C - c)^2 + (b \sin C)^2
$$


Expanding:



$$
a^2 = b^2 \cos^2 C - 2bc \cos C + c^2 + b^2 \sin^2 C
$$

Using , we get:
$$
a^2 = b^2 + c^2 - 2bc\cos C
$$


### When to Use the Law of Cosines

- Given SAS (Side-Angle-Side) to find the unknown side.
- Given SSS (Side-Side-Side) to find the unknown angle.
## Example
### Example: Finding a Missing Side

Suppose you have a triangle where:

- Side a=7a = 7a=7
- Side b=10b = 10b=10
- Angle C = $45^∘$ (between sides a and b)

Find the missing side c:

$$
c^2=7^2+10^2−2(7)(10)cos⁡(45^∘)
$$

$$
c^{2}= =49+100−140×0.7071
$$
$$
c^2=149−98.99
$$
$$
c^2=50.01
$$
$$
c≈\sqrt{50.01​}≈7.07
$$


So, the missing side ccc is approximately **7.07** units.

### Example: Finding a Missing Angle

If you know all three sides of a triangle (a ,b, c,), you can rearrange the formula to find an angle:

$$\cos(C) = \frac{a^2 + b^2 - c^2}{2ab}$$

Let’s say:

- a = 5
- b=6
- c=7

Find angle CCC:

$$cos(C) = \frac{5^2 + 6^2 - 7^2}{2(5)(6)}$$
$$cos(C) = \frac{25 + 36 - 49}{60}$$

$$ cos(C) = \frac{12}{60} = 0.2$$
 
$$ C = \cos^{-1}(0.2)$$
 
$$C≈78.46^∘$$

So, angle CCC is approximately **78.46°**.