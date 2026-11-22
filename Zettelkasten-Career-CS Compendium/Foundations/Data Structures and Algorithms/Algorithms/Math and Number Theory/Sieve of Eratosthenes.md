---
tags:
  - Algorithm
  - FLASHCARD
---
The **Sieve of Eratosthenes** is an ancient and efficient algorithm for finding all prime numbers up to a given limit `n`. It was developed by the Greek mathematician **Eratosthenes** around 200 BCE and is one of the most famous algorithms for finding prime numbers.

# Basic Idea

The algorithm works by iteratively marking the multiples of each prime number starting from 2\. The numbers that remain unmarked after all multiples of primes up to `√n` are processed are the prime numbers.

# Steps of the Algorithm

1. **Initialize**: Create a boolean array `isPrime[]` of size `n+1` (for numbers from `0` to `n`) and set all values to `true`. Initially, we assume that all numbers are prime.

2. **Mark Non-Primes**:

   * Set `isPrime[0]` and `isPrime[1]` to `false`, as 0 and 1 are not prime numbers.

   * Start with the first prime number, `p = 2`. Mark all multiples of `p` as non-prime (`false`).

   * Move to the next number greater than `p` that is still marked as prime (this will be the next prime), and repeat the process until `p * p > n`. This is because all non-prime numbers greater than `n` will already have been marked by smaller primes.

3. **Result**:

   * After marking all the multiples of each prime number, the numbers that remain marked as `true` in the `isPrime[]` array are the prime numbers up to `n`.

# Time Complexity

The time complexity of the Sieve of Eratosthenes is **O(n log log n)**. This is due to the fact that each number `i` is marked by its multiples and each prime number only marks multiples up to `n`.

# Space Complexity

The space complexity is **O(n)** because we need an array of size `n+1` to store the prime status of each number.

# Example

Let's find all prime numbers up to `n = 30` using the Sieve of Eratosthenes.

## Step-by-Step Execution:

1. Initialize the array `isPrime[]`:

```
isPrime[0] = false, isPrime[1] = false, isPrime[2] = true, ..., isPrime[30] = true
```

2.   Start with `p = 2`, and mark all multiples of 2 as `false`:

```
isPrime[4] = false, isPrime[6] = false, isPrime[8] = false, ..., isPrime[30] = false
```

3.   Move to the next number (`p = 3`), and mark all multiples of 3 as `false`:

```
isPrime[6] = false, isPrime[9] = false, isPrime[12] = false, ..., isPrime[30] = false
```

4.   Move to `p = 4`, but since `isPrime[4]` is `false`, skip it.

5. Move to `p = 5`, and mark all multiples of 5 as `false`:

```
isPrime[10] = false, isPrime[15] = false, isPrime[20] = false, isPrime[25] = false, isPrime[30] = false
```

6.   Continue for `p = 7`, marking all multiples of 7\.

7. Now, `p * p > 30`, so we stop.

8. Finally, the remaining `true` values in the `isPrime[]` array correspond to the prime numbers:

```
isPrime[2] = true, isPrime[3] = true, isPrime[5] = true, isPrime[7] = true, isPrime[11] = true, 
isPrime[13] = true, isPrime[17] = true, isPrime[19] = true, isPrime[23] = true, isPrime[29] = true
```

9.   So the primes up to `30` are:

```
2, 3, 5, 7, 11, 13, 17, 19, 23, 29
```

# Java Implementation

Here is a simple Java implementation of the Sieve of Eratosthenes:

```java
import java.util.Arrays;

public class SieveOfEratosthenes {

    public static void sieveOfEratosthenes(int n) {
        // Step 1: Create a boolean array "isPrime[0..n]" and initialize all entries as true.
        // A value in isPrime[i] will be false if i is Not a prime, true if i is a prime.
        boolean[] isPrime = new boolean[n + 1];
        Arrays.fill(isPrime, true);  // Assume all numbers are prime initially

        // Step 2: Set 0 and 1 as non-prime numbers
        isPrime[0] = isPrime[1] = false;

        // Step 3: Use Sieve of Eratosthenes to mark non-primes
        for (int p = 2; p * p <= n; p++) {
            if (isPrime[p]) { // If isPrime[p] is still true, it's a prime
                for (int i = p * p; i <= n; i += p) {
                    isPrime[i] = false; // Mark multiples of p as non-prime
                }
            }
        }

        // Step 4: Print all prime numbers
        System.out.println("Prime numbers up to " + n + " are:");
        for (int i = 2; i <= n; i++) {
            if (isPrime[i]) {
                System.out.print(i + " ");
            }
        }
    }

    public static void main(String[] args) {
        int n = 30;
        sieveOfEratosthenes(n);
    }
}
```

# Explanation of the Code:

1. **isPrime Array**: This array holds the prime status for each number from `0` to `n`. Initially, we assume that all numbers are prime (set to `true`).

2. **Mark Non-Primes**: For each prime `p`, we mark all its multiples (starting from `p * p`) as non-prime by setting `isPrime[i] = false`.

3. **Print Primes**: After processing, the numbers that remain `true` in the `isPrime` array are the prime numbers up to `n`.

# Time Complexity and Optimizations:

* **Time Complexity**: `O(n log log n)`

  * For each prime `p`, we mark its multiples, and this process is efficient because we only need to do it for numbers less than or equal to `√n`.

* **Space Complexity**: `O(n)`

  * We use an array of size `n+1` to store the prime status of each number.

# Optimizations:

* **Start from `p * p`**: Instead of marking multiples of `p` starting from `2p`, we can start from `p * p` because all smaller multiples of `p` will already have been marked by smaller primes.

* **Check only odd numbers for `p` greater than 2**: After processing 2, we can skip even numbers as they are not prime.

---

# Conclusion

The **Sieve of Eratosthenes** is an efficient algorithm for generating a list of primes up to a given limit `n`. Its time complexity of **O(n log log n)** makes it one of the fastest algorithms for finding all primes in a range, and its simplicity and efficiency make it a cornerstone in number-theoretic algorithms.

