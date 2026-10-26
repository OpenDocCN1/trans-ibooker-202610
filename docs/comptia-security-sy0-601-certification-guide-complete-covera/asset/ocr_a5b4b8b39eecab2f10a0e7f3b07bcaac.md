# Hashing and Data Integrity

**Hashing** is where the data inside a document is hashed using an algorithm, such as a **Secure Hash Algorithm Version 1 (SHA1)**, SHA2, SHA3, and MD5. This turns the data inside the file into a long text string known as a hash value; this is also known as a message digest.

While you are hashing the same data, if you copy a file and therefore have two files containing the same data, then hash them with the same hashing algorithm. It will always produce the same hash value. Let’s look at the following examples on hashing and data integrity:

+   - **Verifying Integrity**: During forensic analysis, a scientist takes a copy of the data before investigation. To ensure that they have not tampered with it during the investigation, they will hash the data before starting and then compare the hash to the data when finished. If the hash matches, then they know that the integrity of the data is intact.
+   - **One-Way Function**: For the exam, hashing is a one-way function and cannot be reversed.
+   - **Digital Signatures**: Digital signatures are used to verify the integrity of an email so that you know it has not been tampered with in transit. The private certificate used to sign the email creates a one-way hash function, and when it arrives at its destination, the recipient has already been given a public key to verify that it has not been tampered with in transit. This will be covered in more depth later on in this book.
+   - **RIPEMD**: This is a 128-bit hashing function. RIPEMD has been replaced by RIPEMD-160, RIPEMD-256, and RIPEMD-320. For the exam, you need to know that it can be used to hash data.

# Comparing and Contrasting the Basic Concepts of Cryptography

In this section, we are going to outline the uses of different aspects of cryptography.

# Asymmetric – PKI

Asymmetric keys are obtained from a CA. If you are selling products or services with external entities, then you need to obtain your X509s from a public CA; otherwise, your internal certificates will not be accepted.