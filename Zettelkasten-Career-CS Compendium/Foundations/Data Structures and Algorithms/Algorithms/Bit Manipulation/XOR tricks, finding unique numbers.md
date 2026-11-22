---
tags:
  - Algorithm
  - FLASHCARD
  - ALGO_STUDY
---
The XOR (exclusive OR) operation is one of the most powerful bitwise operations, especially when it comes to solving problems related to finding unique elements in a collection or array. The key property of XOR is that it works based on the concept of **matching bits**:

* **XOR of a number with itself is 0**:  
   `a ^ a = 0`

* **XOR of a number with 0 is the number itself**:  
   `a ^ 0 = a`

* **XOR is commutative and associative**:  
   `a ^ b ^ c = a ^ (b ^ c) = (a ^ b) ^ c`

Given these properties, XOR is useful for problems where we need to identify unique numbers in an array or perform operations like cancelling out repeated elements.

---

# Finding the Unique Number in an Array with Duplicates

A common problem is to find the **one unique number** in an array where all other numbers appear twice. XOR can be used effectively here because pairs of identical numbers cancel out to `0` when XORed together.

## Problem Example:

Given an array where every element appears twice except one, find the element that appears only once.

**Example:**

```java
int[] arr = {4, 3, 2, 4, 3, 2, 7};
```

In this case, the number `7` appears only once, and all the other numbers appear twice.

## XOR Trick:

You can simply XOR all the numbers in the array. The result will be the number that occurs only once, because all the other numbers will cancel each other out due to their paired occurrences.

## Algorithm:

```java
public class UniqueNumber {
    public static int findUnique(int[] arr) {
        int unique = 0;
        for (int num : arr) {
            unique ^= num;  // XOR each element
        }
        return unique;
    }
}
```

**Explanation**:

* Initialize `unique = 0`.

* For each number in the array, XOR it with the current `unique` value.

* Due to the property of XOR (`a ^ a = 0`), all paired numbers will cancel out, and the only remaining value will be the unique number.

**Time Complexity**:

* **O(n)**, where `n` is the size of the array.

**Space Complexity**:

* **O(1)**, since we are using only one extra variable.

## Example Walkthrough:

For the array `{4, 3, 2, 4, 3, 2, 7}`:

1. `unique = 0 ^ 4 = 4`

2. `unique = 4 ^ 3 = 7`

3. `unique = 7 ^ 2 = 5`

4. `unique = 5 ^ 4 = 1`

5. `unique = 1 ^ 3 = 2`

6. `unique = 2 ^ 2 = 0`

7. `unique = 0 ^ 7 = 7`

So, the unique number is `7`.

---

# Finding Two Unique Numbers in an Array

In some variations of the problem, we are given an array where exactly **two numbers appear only once**, and the rest of the numbers appear twice. We need to find the two unique numbers.

## Problem Example:

```java
int[] arr = {4, 3, 2, 4, 3, 7};
```

In this case, the unique numbers are `2` and `7`.

## XOR Trick to Find Two Unique Numbers:

1. XOR all the numbers in the array. The result will be the XOR of the two unique numbers (since the pairs cancel out). Let's call this result `xorResult`.

2. Find any bit that is set (i.e., `1`) in `xorResult`. This bit will be set in one of the two unique numbers but not both.

3. Divide the numbers into two groups based on whether this bit is set or not. XOR the numbers in each group, which will give the two unique numbers.

## Algorithm:

```java
public class TwoUniqueNumbers {
    public static int[] findTwoUnique(int[] arr) {
        int xorResult = 0;
        // Step 1: XOR all elements to get xorResult of the two unique numbers
        for (int num : arr) {
            xorResult ^= num;
        }

        // Step 2: Find the rightmost set bit in xorResult
        int rightmostSetBit = xorResult & -xorResult;

        // Step 3: Divide numbers into two groups and XOR separately
        int unique1 = 0, unique2 = 0;
        for (int num : arr) {
            if ((num & rightmostSetBit) != 0) {
                unique1 ^= num; // Group 1
            } else {
                unique2 ^= num; // Group 2
            }
        }

        return new int[]{unique1, unique2};
    }
}
```

**Explanation**:

* **Step 1**: XOR all the numbers in the array, which gives `xorResult = num1 ^ num2` (XOR of the two unique numbers).

* **Step 2**: Find a bit that is set in `xorResult` using `rightmostSetBit = xorResult & -xorResult`. This bit is the first bit where `num1` and `num2` differ.

* **Step 3**: Divide the array into two groups based on whether the corresponding bit is set or not. XOR the elements in each group, which will give the two unique numbers.

**Time Complexity**:

* **O(n)**, where `n` is the size of the array.

**Space Complexity**:

* **O(1)**, using only a few extra variables.

## Example Walkthrough:

For the array `{4, 3, 2, 4, 3, 7}`:

1. XOR all numbers: `xorResult = 4 ^ 3 ^ 2 ^ 4 ^ 3 ^ 7 = 2 ^ 7 = 5` (the XOR of the two unique numbers).

2. Find the rightmost set bit in `5`: `rightmostSetBit = 5 & -5 = 1`.

3. Divide numbers into two groups:

   * Group 1 (`bit = 1`): `4, 3, 2` → `unique1 = 4 ^ 3 ^ 2 = 2`

   * Group 2 (`bit = 0`): `4, 3, 7` → `unique2 = 4 ^ 3 ^ 7 = 7`

4. The two unique numbers are `2` and `7`.

---

# XOR Tricks for Other Applications

1. **Finding if a number is a power of 2**:  
    A number `n` is a power of 2 if `n & (n - 1) == 0`, and `n > 0`. This trick works because powers of 2 have only one bit set in their binary representation.

    Example:

```java
public boolean isPowerOfTwo(int n) {
    return (n > 0) && (n & (n - 1)) == 0;
}
```

2.   
   **XOR for Subarray Sum Problems**:  
    XOR can be used to find the XOR of a subarray efficiently. If you know the XOR of the entire array, you can calculate the XOR of any subarray by XORing the prefix XORs.

---

# Summary of XOR Tricks

* **Find the unique number**: XOR all the numbers together. The result will be the unique number because pairs cancel out.

* **Find two unique numbers**: XOR all the numbers to get a result that is the XOR of the two unique numbers. Then, find the rightmost set bit and divide the array into two groups based on that bit.

* **Power of 2 check**: Use `n & (n - 1) == 0` to check if `n` is a power of 2\.

XOR is a fast and powerful tool in bit manipulation, especially for problems involving finding unique elements, eliminating duplicates, or performing efficient bitwise operations.

