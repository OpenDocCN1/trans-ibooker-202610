Hash values are known as *one-way hash values* because it is impossible to do the reverse operation of taking the hash value and calculating the message (or data) from it. This is why most vendors hash passwords when they store them on a system—the hash values cannot be retrieved and used to calculate the passwords for the user accounts.

Remember the following key points about hashing:

+   - The hash value has a fixed length that depends on the hashing algorithm. No matter how much data is being hashed, the hashing algorithm always generates the same size hash value.
+   - The hash value is also known as the *message digest*.
+   - Hashing is based on the data, not the header of the file. For example, if you calculate the hash value of a file, then change the name of the file and calculate the hash again, it will be the same hash value. This is so that someone cannot hide or misrepresent a file by renaming it. Technically, if you want to prove it is the same file, you just need to calculate the hash.
+   - Although unlikely, it is technically possible for two different pieces of data to generate the same hash value, depending on the size of the hash value the algorithm creates. For example, a 64-bit hash value can have only so many possible values, so you may get a duplicated hash when a calculation is performed. On the other hand, a 128-bit hash value has many more possible values that can be calculated as hashes. This means there are more opportunities for unique hash values.
+   - *Collision* is the term for when two pieces of different data calculate the same hash value.

# Hashing Algorithms

Just as symmetric encryption algorithms perform the mathematics involved in encrypting information, a hashing algorithm performs the mathematics to calculate hash values from data.

The following are some common hashing algorithms that have been used in recent years:

+   - **Message Digest (MD)** The MD algorithm was created by Ron Rivest and has different versions, such as MD2, MD4, and MD5. The MD5 algorithm is one of the most common hashing algorithms today. It generates a 128-bit hash value.
+   - **Secure Hash Algorithm (SHA)** Created by the National Security Agency, the SHA algorithm has different versions, such as SHA-0, SHA-1, and SHA-2. The most common hashing protocol of the three in use today, SHA-1, creates a 160-bit hash value.
+   - **SHA-256 and SHA-512** These are two newer versions of the SHA algorithm that generate 256-bit and 512-bit hash values, respectively. They are considered to not be susceptible to collision attacks.