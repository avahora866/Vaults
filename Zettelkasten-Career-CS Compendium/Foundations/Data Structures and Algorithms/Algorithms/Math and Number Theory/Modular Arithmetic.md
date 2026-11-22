---
tags:
  - Algorithm
  - FLASHCARD
---
**Modular arithmetic** is a system of arithmetic for integers, where numbers "wrap around" upon reaching a certain value, called the **modulus**. In other words, it deals with the remainder when one number is divided by another.

In modular arithmetic, we say that two integers `a` and `b` are congruent modulo `m` if they have the same remainder when divided by `m`. This is written as:

```
a ≡ b (mod m)
```

This means that when you divide `a` by `m`, the remainder is the same as the remainder when you divide `b` by `m`.

# Basic Concepts

1. **Modulus**:

   * The modulus `m` is the number at which the arithmetic "wraps around."

   * It’s the base or the divisor in modular arithmetic.

2. **Congruence**:

   * If two numbers `a` and `b` leave the same remainder when divided by `m`, they are said to be congruent modulo `m` and written as:

```
a ≡ b (mod m)
```

   *   This means:

```
a % m = b % m
```

3.   **Remainder**:

   * The remainder when dividing `a` by `m` is denoted by `a % m`, and it is the number left over after division.

4. **Addition, Subtraction, and Multiplication Modulo `m`**:

   * Modular arithmetic can also be applied to addition, subtraction, and multiplication. For two numbers `a` and `b` modulo `m`:

	- **Addition**:

```
(a + b) % m = (a % m + b % m) % m
```

- **Subtraction**:

```
(a - b) % m = (a % m - b % m) % m
```

- **Multiplication**:

```
(a * b) % m = (a % m * b % m) % m
```

# Example:

Let’s consider `a = 17`, `b = 5`, and `m = 6`.

1. **Find the remainder when `a` is divided by `m` (i.e., `17 % 6`)**:

```
17 ÷ 6 = 2 remainder 5
So, 17 ≡ 5 (mod 6)
```

2. This means that `17` and `5` leave the same remainder when divided by `6`, so they are congruent modulo `6`.

3. **Addition in Modulo 6**:

    Let's say we want to find `(17 + 5) % 6`:

   * First, reduce `17` and `5` modulo 6:

```
17 % 6 = 5, 5 % 6 = 5
```

   *   Then, add the numbers:

```
(17 + 5) % 6 = (5 + 5) % 6 = 10 % 6 = 4
```

4.   So, `(17 + 5) % 6 = 4`.

5. **Multiplication in Modulo 6**:

    Let's say we want to find `(17 * 5) % 6`:

   * First, reduce `17` and `5` modulo 6:

```
17 % 6 = 5, 5 % 6 = 5
```

   *   Then, multiply the numbers:

```
(17 * 5) % 6 = (5 * 5) % 6 = 25 % 6 = 1
```

6.   So, `(17 * 5) % 6 = 1`.

---

# Properties of Modular Arithmetic

1. **Commutative Property**:

   * Addition and multiplication are commutative under modulo. This means:

```
(a + b) % m = (b + a) % m
(a * b) % m = (b * a) % m
```

2.   **Associative Property**:

   * Addition and multiplication are associative under modulo. This means:

```
(a + (b + c)) % m = ((a + b) + c) % m
(a * (b * c)) % m = ((a * b) * c) % m
```

3.   **Distributive Property**:

   * Multiplication distributes over addition under modulo. This means:

```
(a * (b + c)) % m = ((a * b) + (a * c)) % m
```

4.    **Identity Elements**:

   * The identity element for addition modulo `m` is `0`. That is:

```
(a + 0) % m = a % m
```

   *    The identity element for multiplication modulo `m` is `1`. That is:

```
(a * 1) % m = a % m
```

5.   **Inverses**:

   * The modular multiplicative inverse of `a` modulo `m` is an integer `x` such that:

```
(a * x) % m = 1
```

   *   Not all numbers have an inverse modulo `m`. An inverse exists if and only if `a` and `m` are **coprime** (i.e., GCD(a, m) \= 1).

---

# Applications of Modular Arithmetic

1. **Cryptography**:

   * **RSA encryption** relies heavily on modular arithmetic for public key cryptography.

   * **Hash functions** and **digital signatures** also use modular arithmetic to ensure security.

2. **Computer Science**:

   * Modular arithmetic is often used in algorithms to handle large numbers (e.g., when working with large powers, modular exponentiation is used to prevent overflow).

   * It’s used in hashing algorithms, random number generation, and cryptographic protocols.

3. **Clock Arithmetic**:

   * A common real-life application is in **clock arithmetic**, where time is calculated using modular arithmetic. For example, on a 12-hour clock:

```
(9 + 6) % 12 = 3
```

   *    This represents moving 6 hours forward from 9 o'clock, which results in 3 o'clock.

4. **Number Theory**:

   * Many concepts in number theory, such as **congruences**, **Diophantine equations**, and **Chinese Remainder Theorem**, use modular arithmetic.

---

# Java Example: Modular Arithmetic

Here is an example of how you might implement modular arithmetic operations in Java:

```java
public class ModularArithmetic {

    // Function for addition modulo m
    public static int addMod(int a, int b, int m) {
        return (a + b) % m;
    }

    // Function for multiplication modulo m
    public static int multiplyMod(int a, int b, int m) {
        return (a * b) % m;
    }

    // Function to find the modular inverse of a modulo m using Extended Euclidean Algorithm
    public static int modInverse(int a, int m) {
        int g = gcd(a, m);
        if (g != 1) {
            throw new ArithmeticException("Inverse doesn't exist");
        } else {
            return modInverseUtil(a, m);
        }
    }

    // Helper function for Extended Euclidean Algorithm
    private static int modInverseUtil(int a, int m) {
        int m0 = m;
        int x0 = 0, x1 = 1;
        if (m == 1) return 0;
        while (a > 1) {
            int q = a / m;
            int t = m;
            m = a % m;
            a = t;
            t = x0;
            x0 = x1 - q * x0;
            x1 = t;
        }
        if (x1 < 0) x1 += m0;
        return x1;
    }

    // Function to compute GCD using Euclidean Algorithm
    public static int gcd(int a, int b) {
        while (b != 0) {
            int temp = b;
            b = a % b;
            a = temp;
        }
        return a;
    }

    public static void main(String[] args) {
        int a = 17, b = 5, m = 6;
        System.out.println("Addition Modulo " + m + ": " + addMod(a, b, m));  // (17 + 5) % 6
        System.out.println("Multiplication Modulo " + m + ": " + multiplyMod(a, b, m));  // (17 * 5) % 6

        int inverse = modInverse(a, m);
        System.out.println("Modular Inverse of " + a + " modulo " + m + " is: " + inverse);
    }
}
```

# Output:

```
Addition Modulo 6: 4
Multiplication Modulo 6: 1
```

---

# Conclusion

**Modular arithmetic** is an essential concept used in various fields, including computer science, cryptography, and number theory. It simplifies operations on large numbers and provides the foundation for algorithms like RSA encryption and many hashing techniques. Understanding modular arithmetic is key to solving many algorithmic problems efficiently.

