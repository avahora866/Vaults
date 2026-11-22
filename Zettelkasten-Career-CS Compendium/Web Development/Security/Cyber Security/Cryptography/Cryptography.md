Two main types of encryption

# Symmetric Encryption (The "Shared Secret")
This is the oldest form of encryption (used since Julius Caesar).
- The Logic: There is one single key.
- Action: The same key locks the box, and the same key unlocks it.
- Example Algorithms: AES (Advanced Encryption Standard).
- Speed: Extremely fast.

The Fatal Flaw (The Key Exchange Problem): If I want to send you a secret message using AES, I must first give you the Key.

How do I send you the Key?
- If I email it to you, the hacker intercepts it.
- If I shout it, the hacker hears it.

Paradox: To send a secret securely, we first need a secure way to share the secret.

# Asymmetric Encryption (Public/Private Keys)
This was the mathematical breakthrough of the 1970s that saved the Internet. Instead of one key, every user generates two keys that are mathematically linked.

- The Public Key: You give this to everyone. You post it on your website. It is like an Open Padlock. Anyone can use it to lock a box, but no one (not even the person who locked it) can unlock it.
- The Private Key: You keep this secret. Never share it. It is the only Key that can open the padlock.

The Workflow:
- You want to send Me a secret.
- You download my Public Key (Padlock) from the internet.
- You put your message in a box and lock it with my Public Key.
- You ship the box through the dangerous internet.
- The Hacker steals the box. He tries to open it. He cannot. He tries to use the Public Key to reverse it. He cannot.
- I receive the box.
- I use my Private Key to unlock it.

# Digital Signatures
Asymmetric Encryption works in reverse, too. This is how we prove Identity (Digital Signatures).
If I lock a message using my Private Key, it can only be unlocked using my Public Key.

Why would I do this? If you receive a message from me, and you successfully unlock it with my Public Key, you know a mathematical fact:
- Fact: The message must have been locked by the Professor's Private Key.
- Conclusion: Since only the Professor has that Private Key, the message must be from the Professor.

# Hashing (The One-Way Trip)
The goal of encryption is to hide data so it can be decrypted later. It is a two-way street. Hashing is different. It is a one-way street.

Hashing is a Blender: You put a document inside and turn it on. You get a "slurry" (the Hash) out.
- Crucial Rule: You cannot un-blend the slurry to get the document back. It is mathematically impossible.

Why destroy data? (Password Security) 
If you cannot reverse it, why is it useful? Verifying Passwords.

When you create an account on a website and type your password (password123):
1. The Server does not save password123 in the database. (If it did, a SQL Injection would reveal everyone's passwords).
2. The Server runs the password through a Hashing Algorithm (like SHA-256).
    - password123 $\rightarrow$ ef92b778bafe771e89245b89ecbc08a4...
3. The Server saves this "Hash" string.

When you log in next time:
1. You type password123.
2. The Server hashes it again.
3. It compares the new hash to the saved hash.
4. If they match, it knows you typed the right password, without the server ever knowing what your actual password is.

## Rainbow Tables
Because Hashing is standard math, password123 always results in the exact same hash: ef92b7....

Hackers know this. They pre-calculate the hashes for millions of common passwords (dictionary words, "123456", etc.) and store them in massive spreadsheets called Rainbow Tables.

If a Hacker steals the database and sees ef92b7..., they just look it up in their table and see: "Ah, that hash corresponds to 'password123'."

### The Defense: Salting
To defeat Rainbow Tables, we use Salt. Salt is a random string of characters added to the password before it is hashed.

- User A: password123 + Salt(Xy9#) $\rightarrow$ Hash(29a8c...)
- User B: password123 + Salt(Bk2@) $\rightarrow$ Hash(88f1q...)

Now, even though both users have the same password, they have completely different hashes in the database. The Hacker's pre-calculated table is useless.