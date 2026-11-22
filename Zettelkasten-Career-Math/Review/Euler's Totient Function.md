---
tags:
---
Euler's Totient Function, denoted as **ϕ(n)**, is a function in number theory that counts the number of positive integers up to n that are relatively prime to n. Two numbers are relatively prime (or coprime) if their greatest common divisor (gcd) is 1.

### Definition

ϕ(n) = The number of integers k in the range 1 ≤ k ≤ n for which gcd(k, n) = 1.

### How It Works

- If n is a **prime number**, then ϕ(n) = n - 1, because all numbers less than n are relatively prime to n.  
    Example: ϕ(7) = 6 because 1, 2, 3, 4, 5, and 6 are all relatively prime to 7.
    
- If n is a **product of two prime numbers**, p and q, then:  
    ϕ(n) = (p - 1) * (q - 1)  
    This is because multiples of p or q are not relatively prime to n.
    

### Formula

For a general number n with prime factorization:  
n = p1^k1 * p2^k2 * ... * pm^km  
(where p1, p2, ..., pm are distinct prime factors), the formula for Euler's Totient Function is:  
ϕ(n) = n * (1 - 1/p1) * (1 - 1/p2) * ... * (1 - 1/pm)

### Examples

1. **ϕ(9):**
    
    - 9 = 3^2
    - The numbers less than 9 are: 1, 2, 3, 4, 5, 6, 7, 8
    - The numbers relatively prime to 9 are: 1, 2, 4, 5, 7, 8
    - Count = 6, so ϕ(9) = 6
2. **ϕ(10):**
    
    - 10 = 2 * 5
    - Using the formula: ϕ(10) = 10 * (1 - 1/2) * (1 - 1/5) = 10 * (1/2) * (4/5) = 4
    - The numbers relatively prime to 10 are: 1, 3, 7, 9
3. **ϕ(12):**
    
    - 12 = 2^2 * 3
    - Using the formula: ϕ(12) = 12 * (1 - 1/2) * (1 - 1/3) = 12 * (1/2) * (2/3) = 4
    - The numbers relatively prime to 12 are: 1, 5, 7, 11

### Why is it Important?

- In **RSA encryption**, ϕ(n) is used to compute the private key because it helps in determining the modular inverse needed for decryption.
- In **Number Theory**, it is used to study modular arithmetic and the properties of numbers.

Would you like more examples or details on how it's used in cryptography?