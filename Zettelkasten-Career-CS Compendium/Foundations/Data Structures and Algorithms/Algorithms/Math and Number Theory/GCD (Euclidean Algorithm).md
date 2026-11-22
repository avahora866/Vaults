---
tags:
  - Algorithm
  - ALGO_STUDY
---
The **Greatest Common Divisor (GCD)** of two integers is the largest positive integer that divides both of them without leaving a remainder. In other words, the GCD is the biggest number that can evenly divide both numbers.

For example:

* GCD(24, 36\) \= 12, because 12 is the largest number that divides both 24 and 36\.

The **Euclidean Algorithm** is an efficient method for computing the GCD of two numbers. It works by repeatedly reducing the problem using division and remainders until the GCD is found.

---

# Steps of the Euclidean Algorithm

The Euclidean Algorithm is based on the following principle:

For two integers `a` and `b` (where `a ≥ b`), the **GCD of a and b** is the same as the **GCD of b and the remainder when a is divided by b**.

Mathematically, this is expressed as:

```
GCD(a, b) = GCD(b, a % b)
```

Where `%` is the modulus operator, which gives the remainder when `a` is divided by `b`.

# Algorithm

1. **Start with two numbers `a` and `b`** (where `a >= b`).

2. **Divide `a` by `b`**, and compute the remainder: `remainder = a % b`.

3. **Replace `a` with `b`**, and replace `b` with the remainder.

4. **Repeat** the process until `b` becomes 0\. When this happens, the value of `a` is the GCD.

---

# Example: GCD of 24 and 36

Let's find the GCD of 24 and 36 using the Euclidean Algorithm.

1. Start with `a = 36`, `b = 24`.

   * Compute `36 % 24 = 12` (remainder)

   * Now, replace `a` with `b`, and `b` with the remainder: `a = 24`, `b = 12`.

2. Now, `a = 24`, `b = 12`.

   * Compute `24 % 12 = 0` (remainder)

   * Now, replace `a` with `b`, and `b` with the remainder: `a = 12`, `b = 0`.

3. Since `b` is now 0, the algorithm stops, and the GCD is `a = 12`.

Thus, **GCD(24, 36\) \= 12**.

---

# Why Does the Euclidean Algorithm Work?

The Euclidean Algorithm works because of the **Division Lemma**, which states that for any two integers `a` and `b`, there exist unique integers `q` (quotient) and `r` (remainder) such that:

```
a = b * q + r
```

This implies that the GCD of `a` and `b` is the same as the GCD of `b` and `r` because any divisor of both `a` and `b` must also divide `r`. Similarly, the GCD of `b` and `r` is the same as the GCD of `a` and `b`.

---

# Time Complexity

The time complexity of the Euclidean Algorithm is **O(log(min(a, b)))**. This is because, in each step, the size of the numbers involved decreases rapidly (roughly halved), and it takes logarithmic steps to reduce the numbers down to 0\.

---

# Java Implementation of Euclidean Algorithm

Here is a simple Java implementation of the **Euclidean Algorithm** to compute the GCD of two numbers:

```java
public class EuclideanAlgorithm {

    // Function to compute the GCD of a and b
    public static int gcd(int a, int b) {
        // Continue until b becomes 0
        while (b != 0) {
            int remainder = a % b;  // Compute the remainder
            a = b;                   // Update a to b
            b = remainder;           // Update b to the remainder
        }
        return a;  // When b is 0, the GCD is a
    }

    public static void main(String[] args) {
        int a = 24;
        int b = 36;
        int result = gcd(a, b);  // Calculate GCD of 24 and 36
        System.out.println("GCD of " + a + " and " + b + " is " + result);
    }
}
```

# Explanation of the Code:

* The `gcd()` function takes two integers `a` and `b` as input.

* It enters a `while` loop and repeatedly calculates the remainder (`a % b`).

* The values of `a` and `b` are updated in each iteration: `a` becomes `b`, and `b` becomes the remainder.

* The loop continues until `b` becomes 0, at which point `a` holds the GCD of the two numbers.

# Sample Output:

For the input `a = 24` and `b = 36`, the output will be:

```
GCD of 24 and 36 is 12
```

---

# Edge Cases

1. **When one of the numbers is 0**:

   * The GCD of any number and `0` is the number itself. For example:

     * GCD(0, 5\) \= 5

     * GCD(7, 0\) \= 7

2. **When both numbers are 0**:

   * The GCD of `0` and `0` is undefined, but by convention, we may return 0 or raise an exception.

---

# Conclusion

The **Euclidean Algorithm** is an efficient and simple method to compute the **GCD** of two numbers. Its time complexity is logarithmic, making it a very fast algorithm even for large numbers. This algorithm is widely used in number theory and computer science, especially in cryptography and simplifying fractions.

