---
tags:
  - Algorithm
  - ALGO_STUDY
---
**Bit Manipulation** refers to the process of performing operations on binary representations of numbers. Since computers use binary (0s and 1s) to represent data, bit manipulation allows us to manipulate individual bits of a number, offering a fast and memory-efficient way to solve certain types of problems.

In **bit manipulation**, you typically work with **bitwise operators** to perform operations like setting, clearing, toggling, or testing individual bits.

# Bitwise Operators in Java

Here are the common **bitwise operators**:

1. **AND (`&`)**: Performs a bitwise AND operation between corresponding bits of two numbers.

   * **Result**: A bit is `1` if both corresponding bits are `1`, otherwise `0`.

2. Example:

```java
5 & 3 = 1    // 0101 & 0011 = 0001
```

3.   **OR (`|`)**: Performs a bitwise OR operation between corresponding bits of two numbers.

   * **Result**: A bit is `1` if at least one corresponding bit is `1`, otherwise `0`.

4. Example:

```java
5 | 3 = 7    // 0101 | 0011 = 0111
```

5.   **XOR (`^`)**: Performs a bitwise XOR (exclusive OR) operation.

   * **Result**: A bit is `1` if the corresponding bits are different, otherwise `0`.

6. Example:

```java
5 ^ 3 = 6    // 0101 ^ 0011 = 0110
```

7.   **NOT (`~`)**: Performs a bitwise NOT (complement) operation.

   * **Result**: Flips each bit (turns `1` to `0` and `0` to `1`).

8. Example:

```java
~5 = -6      // 0101 -> 1010 in 2's complement
```

9.   **Left Shift (`<<`)**: Shifts the bits of a number to the left by a specified number of positions, adding `0` bits from the right.

   * **Result**: Equivalent to multiplying the number by `2^n`, where `n` is the number of positions shifted.

10. Example:

```java
5 << 1 = 10  // 0101 << 1 = 1010 (shift left by 1)
```

11.   **Right Shift (`>>`)**: Shifts the bits of a number to the right by a specified number of positions.
	* **Result**: Equivalent to dividing the number by `2^n` (and rounding down), where `n` is the number of positions shifted.
12. Example:

```java
5 >> 1 = 2   // 0101 >> 1 = 0010 (shift right by 1)
```

13.   **Unsigned Right Shift (`>>>`)**: Like the right shift, but it always fills with `0`s from the left, even for negative numbers.

    * **Result**: Shifts the bits to the right and adds `0` bits from the left.

14. Example:

```java
-5 >>> 1 = 2147483642
```

---

# Common Bit Selection, Bubble, InsertionManipulation Techniques

1. **Checking if a number is even or odd** You can check if a number is even or odd using the bitwise **AND** operation with `1`.

   * If the least significant bit (LSB) is `0`, the number is even; if it's `1`, the number is odd.

2. Example:

```java
int num = 5;
if ((num & 1) == 0) {
    System.out.println("Even");
} else {
    System.out.println("Odd");
}
```

3.   **Clearing the rightmost set bit** This can be done using the expression `num & (num - 1)`, which clears the rightmost `1` bit.

Example:

```java
int num = 12; // 1100 in binary
num = num & (num - 1); // 1100 & 1011 = 1000 (clears the rightmost set bit)
```

4.   **Counting the number of set bits (1's)** You can count the number of `1` bits (also called **Hamming Weight**) in a number using the expression `num & (num - 1)`, which removes the rightmost `1` bit in each iteration.

Example:

```java
int countSetBits(int num) {
    int count = 0;
    while (num > 0) {
        num = num & (num - 1); // Clears the rightmost set bit
        count++;
    }
    return count;
}
```

5.   **Checking if a number is a power of 2** A number is a power of 2 if and only if it has exactly one `1` bit in its binary representation. This can be checked using the condition `num & (num - 1) == 0`.

Example:

```java
boolean isPowerOfTwo(int num) {
    return (num > 0) && ((num & (num - 1)) == 0);
}
```

6.   **Swapping two numbers without using a temporary variable** You can swap two numbers `a` and `b` using XOR.

Example:

```java
int a = 5, b = 10;
a = a ^ b;
b = a ^ b; // Now b = 5
a = a ^ b; // Now a = 10
```

7.   **Setting a specific bit to 1** To set a specific bit at position `i` to `1`, you can use the **OR** operator with a mask where only the `i`th bit is `1`.

Example:

```java
int num = 4; // 0100 in binary
int i = 1;
num = num | (1 << i); // Set the 1st bit to 1
// num = 6 (0110 in binary)
```

8.   **Clearing a specific bit to 0** To clear (set to `0`) a specific bit at position `i`, you can use the **AND** operator with a mask where only the `i`th bit is `0`.

Example:

```java
int num = 7; // 0111 in binary
int i = 1;
num = num & ~(1 << i); // Clear the 1st bit
// num = 5 (0101 in binary)
```

9.   **Toggling a specific bit** To toggle a specific bit (flip it between `0` and `1`), you can use the **XOR** operator.

Example:

```java
int num = 4; // 0100 in binary
int i = 1;
num = num ^ (1 << i); // Toggle the 1st bit
// num = 6 (0110 in binary)
```

10.   **Reversing the bits of a number** You can reverse the bits of a number by shifting and OR-ing the bits into a new number.

Example:

```java
int reverseBits(int num) {
    int result = 0;
    while (num > 0) {
        result = (result << 1) | (num & 1);
        num >>= 1;
    }
    return result;
}
```

---

# Applications of Bit Manipulation

1. **Efficient Computation**: Bit manipulation can be used to efficiently solve problems related to arithmetic operations, set operations, and number theory, where operations need to be performed on individual bits.

2. **Masking**: This is used to extract or set specific bits in a number. For example, **setting**, **clearing**, **toggling**, or **testing** particular bits in a number.

3. **Network Programming**: Bitwise operations are often used in networking to deal with IP addresses and subnetting.

4. **Cryptography**: Many cryptographic algorithms rely on bitwise operations for operations like encryption and hashing.

5. **Binary Search Optimization**: Bit manipulation can optimize binary search algorithms by reducing the need for multiplication/division operations.

6. **Memory-efficient data structures**: Some data structures like **Bloom Filters** and **Bit Arrays** rely on bit manipulation for efficient space usage and fast lookup operations.

---

# Summary of Common Bitwise Operations

| Operation            | Symbol | Example    | Description                           |
| -------------------- | ------ | ---------- | ------------------------------------- |
| AND                  | `&`    | `5 & 3`    | Sets bit to `1` if both bits are `1`  |
| OR                   | \`     | \`         | \`5                                   |
| XOR                  | `^`    | `5 ^ 3`    | Sets bit to `1` if bits are different |
| NOT                  | `~`    | `~5`       | Inverts all bits                      |
| Left Shift           | `<<`   | `5 << 1`   | Shifts bits left (multiply by 2\)     |
| Right Shift          | `>>`   | `5 >> 1`   | Shifts bits right (divide by 2\)      |
| Unsigned Right Shift | `>>>`  | `-5 >>> 1` | Shifts bits right and fills with 0    |

---

Bit manipulation allows us to solve problems efficiently by directly manipulating the underlying binary representations of numbers. It’s particularly useful in scenarios that require low-level optimization, such as competitive programming, system programming, and working with hardware.

