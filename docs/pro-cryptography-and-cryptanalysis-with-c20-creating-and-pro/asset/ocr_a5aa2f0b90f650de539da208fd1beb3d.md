![](bbox=[0.12, 0.11, 0.9, 0.38])

# Figure 23-3. KPA representation

# Chosen-Plaintext Attack

In a chosen-plaintext attack (CPA), the cryptanalyst has the ability to select the plaintext that has been sent encrypted using an encryption algorithm and they can observe how the ciphertext is generated. This can be observed as an active model where the cryptanalyst has the chance to select the plaintext and to realize the encryption.

Based on the ability to select and pick any plaintext, the cryptanalyst has the chance to observe vital details about the ciphertext, which gives them a strong advantage in understanding how the algorithm works inside and the chance to get the secret key.

A professional cryptanalyst will have a strong database that contains known plaintexts, ciphertexts, and possible keys. In Listing 23-1 and Figure 23-5, we have provided an example of generating possible keys automatically. It is a very simple example for illustrating how possible keys can be generated. They can be used with the pairs to determine the cipher text input (see Figure 23-4).