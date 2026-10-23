# Ciphertext-Only Attack

A ciphertext-only attack (COA) represents the weakest attack because it can easily be used by the cryptanalyst due to the fact that they just encoded the message.

The attacker-cryptanalyst has access to a set of ciphertexts. The attack is fully successful if the corresponding plaintexts are deduced together with the key.

In this type of attack (see Figure 23-2), the attacker/cryptanalyst will be able to observe the ciphertext. Everything that the cryptanalyst will see is represented by a set of scrambled and nonsense characters that create the output based on the encryption process.

![](bbox=[0.082, 0.328, 0.878, 0.597])

Figure 23-2. COA representation

# Known-Plaintext Attack

The known-plaintext attack (KPA) helps the cryptanalyst to generate the ciphertext based on the fact that he is aware of the ciphertext.

The cryptanalyst follows a simple procedure by selecting the plaintext, but they will observe the pair that is compounded from the plaintext and ciphertext. The chance of success is better compared to COA. Simple ciphers are quite vulnerable to this attack. See Figure 23-3.