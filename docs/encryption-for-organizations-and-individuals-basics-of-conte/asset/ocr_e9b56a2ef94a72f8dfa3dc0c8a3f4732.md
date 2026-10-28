# Countermeasures Against Rainbow Table Attacks:

+   - Users: pick strong (i.e., long and complicated) passwords only. System administrators: choose to implement newer and more secure hashing algorithms, such as *SHA-256* or *SHA-3*. Also, secure your hash value database with every means at your disposal, including budgeting for physical security.
+   - Salting and/or peppering as described in Chapter 3 (e.g., adding random data to hash values and passwords). This is usually a rather devastating response against these types of attacks.
+   - Implementing *key stretching*, which refers to adding multiple passes of salted hash data into the hash function.

# Replay Attack

A *replay attack* is a type of simple man-in-the-middle or eavesdropping approach. This type of attack consists of a malicious party retransmitting valid credentials into a system, thereby gaining access to it. In theory, a replay attack can be therefore used to circumvent even the strongest of encryption schemes.

# Countermeasures Against Replay Attacks:

+   - Using a *session ID* approach, where each run of a program is assigned a random, one-time token; a malicious party simply couldn’t replicate this to commence a replay attack later.
+   - Using *timestamps*, where “the lines are open” only for a brief time window.