---
tags:
  - Security
  - FORMAT
---
# Hashing

 - Definition: [[Theory/Hashing]] is a process that converts data into a fixed-size string of characters, typically a hash value or checksum. It is a one-way function, meaning that it is computationally infeasible to reverse the process.  
 - Common Hashing Algorithms:  
   - SHA (Secure Hash Algorithm): SHA-256, SHA-512 (more secure than MD5).  
   - MD5: A legacy algorithm, now considered insecure due to vulnerabilities.  
 - Role in Authentication/Authorization:  
   - Password Storage: In authentication systems, passwords are hashed before storing them in a database, ensuring the original password is never saved in plaintext.  
   - Data Integrity: Hashing is used to verify that data hasn't been tampered with by comparing hash values before and after transmission.

# Encryption/Decryption

 - Symmetric Encryption:  
   - Involves a single shared key for both encryption and decryption.  
   - AES (Advanced Encryption Standard) is commonly used in securing communication.  
   - Use in Authentication/Authorization: Symmetric encryption is often used to encrypt sensitive data in transit (e.g., HTTPS, VPNs) and protect sessions in web applications.  
 - Asymmetric Encryption:  
   - Involves two keys: a public key for encryption and a private key for decryption.  
   - [[RSA (Rivest–Shamir–Adleman)|RSA (Rivest-Shamir-Adleman)]] is a widely used algorithm for public-key cryptography.
   - Use in [[Authentication]]/Authorization: 
     - Digital Signatures: Used for identity verification, ensuring the sender's identity and message integrity.  
     - SSL/TLS: Asymmetric encryption secures data exchanges over the internet (e.g., HTTPS).  
	 - Use in [[Authentication]] Systems:  
	   - SSL/TLS #LINK: Ensures secure communication between clients and servers by encrypting the connection, essential in authentication workflows like login pages.  
	   - JWT (JSON Web Tokens): Often uses asymmetric encryption to securely transmit user claims (e.g., authentication tokens).

# Digital Signatures

 - Definition: A digital signature uses a combination of hashing and asymmetric encryption to verify the authenticity and integrity of a message or document.  
 - How It Works:  
   - The sender creates a hash of the message and encrypts it with their private key. The receiver uses the sender’s public key to verify the hash and authenticate the sender's identity.  
 - Use in [[Authentication]]/Authorization:  
   - OAuth 2.0 and JWT: Digital signatures are used to sign tokens and ensure that tokens haven’t been altered, verifying the identity of the user or service.  
   - Code Signing: Ensures that software or code hasn't been altered after it was signed by a developer or organization.

# Cryptography in Authentication and Authorization Systems

 - [[Authentication]]:  
   - Passwords & Multi-Factor Authentication (MFA): When users authenticate, passwords are hashed, and tokens may be signed with a private key to ensure the request's authenticity. 
   - OAuth 2.0: Involves tokens that are signed and sometimes encrypted to ensure they can only be used by the intended recipient, adding an additional layer of security to the authentication process.  
 - Authorization:  
   - [[JWT|JWT (JSON Web Tokens)]]: Often used to convey claims, roles, and permissions that determine what an authenticated user is authorized to do.  
   - Public Key Infrastructure (PKI): In systems where strong authentication is required, PKI certificates can be used to authorize users or devices by verifying their public key against a trusted certificate authority (CA).

# Protocols Using Cryptography

 - SSL/TLS: Secures web traffic, ensuring that communication between a client (browser) and a server is encrypted and verified.  
 - OAuth 2.0: A protocol that often uses tokens (JWT) signed by a private key to enable secure delegated access between clients and servers.  
 - [[JWT]]: Securely transmits claims (such as user identity) in a compact, URL-safe manner, often signed using a secret or private key.

# Key Concepts to Remember:

 - Symmetric Encryption (same key for encryption and decryption) is fast but requires a secure key distribution mechanism.  
 - Asymmetric Encryption (public/private key pairs) is slower but allows for secure key exchange and identity verification without sharing keys in advance.  
 - Digital Signatures verify both the identity of the sender and the integrity of the data, enabling trust in authentication and authorization systems.  
 - [[Theory/Hashing]] ensures data integrity, making it impossible to retrieve the original data and commonly used in password verification and data verification.

