
This function can be used to verify the integrity of the data by comparing the hash value of the original data with the hash value of the data at some later point in time.

Using a cryptographic hash to detect tampering of a message involves the following three steps (as illustrated in [Figure 3.28](#c03.xhtml#c03-fig-0028)):
1. Generate the hash value (`H[1]`) by applying the hash function to the original block of data or message (`M[1]`).
2. Generate the hash value (`H[2]`) by applying the hash function to the block of data or message (`M[2]`) to be verified.
3. If (`H[1]`) is the same as (`H[2]`), then that indicates that the two messages are likely to be identical and have not been tampered with. As we shall shortly see, however, the key question is “how likely?” What degree of confidence should we have that two messages are identical if their hash values are identical?

![FIGURE 3.28 Steps for using a cryptographic hash to detect tampering of a message ¶ From NIST Special Publication SP800-175B, “Guideline for Using Cryptographic Standards in the Federal Government: Cryptographic Mechanisms”](images/c03f028.png)

**[FIGURE 3.28](#c03.xhtml#R_c03-fig-0028) Steps for using a cryptographic hash to detect tampering of a message** ¶ From NIST Special Publication SP800**\-**175B, “Guideline for Using Cryptographic Standards in the Federal Government: *Cryptographic Mechanisms”*

To understand hashing, let's consider a simple example, the checksum. A *checksum* is a mathematical function that takes a block of data and calculates a number (the checksum) in a manner such that any single change to the block of data will cause the checksum number to change. As the name implies, a checksum is typically calculated by taking the sum of each byte in the message, as an unsigned integer, and ignoring any overflow.

When the primary threat was from noise or other defects in the communications channel, data packets carried a checksum that could be used by the receiving party to determine if the data had been corrupted in transit. The receiving party recalculated the checksum for the packet, compared it with the checksum calculated by the sender and appended to the message, and if they did not match, the recipient would know the message had been received with errors.

In a situation in which the changes to the message are random and not malicious, a simple checksum can be sufficient to protect the integrity of the message. But imagine a situation in which a threat actor wants to tamper with the message. Instead of an order to transfer $100, the malicious agent wants to change the message to transfer $100,000. Obviously, if just the amount field is changed, then the checksum is unlikely to match, and his tampering will probably be caught.

That's not a problem for our attacker, however, who understands the algorithm used to calculate the checksum and can further tamper with the message, either by making other changes to the message content so that the checksum remains the same or by calculating the new checksum and overwriting the original checksum. With either approach, the checksum will validate, and the fraudulent message to transfer $100,000 will be accepted as valid.

For example, consider the following transmission, where `[0x116c]` is the checksum appended to the message by the sender:

```
TRANSFER $100.00 TO JANE DOE FOR OFFICE AND MISC EXPENSES IN DECEMBER [0x116c]
```

Our attacker wants to send a slightly different message (but copies the checksum from the original message):

```
TRANSFER $100,000.00 TO JANE DOE FOR OFFICE AND MISC EXPENSES IN DECEMBER [0x116c]
```

The problem is the actual checksum of the new message (`0x1228`) doesn't match the claimed checksum (`0x166c`), so his tampering would be detected.

One possible attack would be to make other changes to the message so that the checksum is the same as the original message:

```
TRANSFER $100,000.00 TO JANE DOE FOR OFFICE MISC EXPENSES IN DECEMBER7 [0x116c]
```

which has the same `0x116c` checksum as the original. Alternatively, the attacker could overwrite the checksum that is appended to the message to be the checksum of the message after being tampered with:

```
TRANSFER $100,000.00 TO JANE DOE FOR OFFICE AND MISC EXPENSES IN DECEMBER [0x1228]
```

Clearly, using a simple checksum and including it in the message is not going to work to detect malicious tampering, but it is useful to help avoid nonmalicious errors in transmission or data entry. Checksums are often used to prevent accidental errors for credit card, government ID number, and other types of verification processes. One of the most commonly used algorithms is the Luhn algorithm, which is specified by ISO/IEC 7812-1.

When checksums aren't sufficient, we need a method of verifying message integrity that the attacker cannot easily circumvent. To protect the integrity of data, we need a hashing function sufficiently complex that it is essentially not possible to create a message with the same hash as another message. When two different messages generate the same hash value, it is called a *collision*, so we need a hashing function that makes it almost impossible to create an intentional collision.

In our previous example, two factors made it trivial to create a collision:
* The simplicity of the algorithm made it easy to determine what changes need to be made to the modified message to change the hash (checksum) to match the original message's.
* Even if the algorithm had been complex, the shortness of the hash (16 bits) means that, with only 65,536 possible hash values, it would not take long to repeatedly make random changes to the end of the message until one happened upon a collision.

Cryptographically secure hash algorithms must therefore be sufficiently complex that it is not possible to determine what changes need to be made to a message to create a specific hash result, and the length of the hash value (i.e., number of bits) must be sufficiently large to make brute-force attacks computationally infeasible.

One of the organizations responsible for researching and recommending suitable cryptographic algorithms is NIST. One of their guidelines, FIPS 202, recommends a family of algorithms known as Secure Hash Algorithm-3 (`SHA-3`). The `SHA-3` set of functions includes hashing functions that produce hash values varying in length from 224 to 512 bits.

Using `SHA-3`, we can compute a hash value that is very difficult to attack by creating an intentional collision — but how do we protect against the attacker simply calculating a new hash value for the changed message and overwriting the original hash? For this, we need to use a cryptographic hash that incorporates a secret key known only to the sender and receiver. The value of the hash depends not only on the contents of the message, but on this secret key.

One method, known as Hash-based Message Authentication Code (`HMAC`), was developed by Bellare, Canetti, and Krawczyk and formalized in RFC-2104. This algorithm concatenates a secret key (which has been XORed with a string to pad it to a fixed length) and hashes that. It then takes that hash, combines it with the key again, and hashes it a second time, producing the `HMAC` value (see [Figure 3.29](#c03.xhtml#c03-fig-0029)). If the attacker changes the message without knowing the secret key `K`, the `HMAC` value will not match. This enables the recipient to verify both the integrity and origin (i.e., authentication or nonrepudiation) of the message.
