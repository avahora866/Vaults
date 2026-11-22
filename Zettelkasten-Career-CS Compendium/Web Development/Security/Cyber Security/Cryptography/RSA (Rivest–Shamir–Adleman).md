---
tags:
  - Security
  - FORMAT
---
RSA (Rivest–Shamir–Adleman) is one of the most widely used public-key cryptosystems. It is used for secure data transmission and is based on the mathematical difficulty of factoring the product of two large prime numbers. Here's how it works:

# Key Generation

RSA involves two keys:

- Public Key (e, n): Used for encryption and is shared with everyone.
- Private Key (d, n): Used for decryption and is kept secret.

The key generation steps are:

1. Select two large prime numbers: p and q.
2. Calculate n:  
    $n = p\;  q$  
    n is used as a modulus for both the public and private keys.
3. Calculate Euler's Totient Function (ϕ(n)):  
    $ϕ\;(n)\;\; =\; (p\; -\; 1)\;\;  (q\; -\; 1)$
4. Choose a public exponent (e):
    - e should be such that $1 < e < ϕ(n)$
    - e is typically chosen as a prime number like 65537 because it makes encryption faster.
    - e and $ϕ(n)$ should be coprime (i.e., $gcd(e, ϕ(n)) = 1$).
5. Calculate the private exponent (d):  
    - d is the modular inverse of e mod ϕ(n), which means:  
    - $(d  e)\;\;  \%\;\;  ϕ(n) = 1$

The public key is (e, n) and the private key is (d, n).

# Encryption

To encrypt a message M:

6. Convert the message into an integer m, where m < n.
7. Calculate the ciphertext c using the public key:  
    $c = m^e\;\; \%\;\; n$

# Decryption

To decrypt the ciphertext c:

8. Calculate the original message m using the private key:  
    m = c^d % n
9. Convert m back to the original message.

# Example

Let's go through a simplified example:

- Choose two primes: p = 61, q = 53
- Calculate n: n = 61  53 = 3233
- Calculate ϕ(n): ϕ(n) = (61 - 1)  (53 - 1) = 3120
- Choose e: Let's use e = 17 (commonly used and satisfies gcd(17, 3120) = 1)
- Calculate d: d is the modular inverse of 17 mod 3120, which is 2753  
    (You can find this using the Extended Euclidean Algorithm)

Public key: (17, 3233)  
Private key: (2753, 3233)

If Alice wants to send Bob the message m = 65:

- She encrypts it using Bob's public key:  
    c = 65^17 % 3233 = 2790
- Bob decrypts it using his private key:  
    m = 2790^2753 % 3233 = 65

# Why is RSA Secure?

The security of RSA comes from the fact that, given n, it is extremely difficult to factor it into p and q if the numbers are sufficiently large (e.g., 2048-bit keys). The hardness of this factorization problem underpins the security of the RSA algorithm.

If you want to dive deeper into any part, let me know!

 Minor Notes
Public Exponent (in RSA Encryption)

In the context of RSA encryption, the public exponent (denoted as e) is part of the public key used to encrypt messages or verify digital signatures. It is paired with the modulus n to form the public key: (e, n).

Key Properties:

e must be:

A positive integer.

Coprime with ϕ(n), where ϕ(n) is Euler's Totient Function of the modulus n. This ensures that e has a modular inverse, which is required for decryption.

Usually a small prime number to make encryption faster.



Common Choices for e:

65537 (2^16 + 1) is commonly used because:

It is a prime number.

It is large enough to provide security.

It is small enough to allow efficient encryption and verification.



Why is it Important?

In RSA, the public exponent is used in the encryption process:

where:

 = message (as a number)

 = ciphertext


It is also used to verify digital signatures.


Example:

If the public key is (e = 65537, n = 3233), then to encrypt a message m = 123: