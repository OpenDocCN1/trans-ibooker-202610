```
//** define number of known pairs
pairs_of_numbers = 8;

//** look for inputs that meet specific characteristics
generate_characters_data(4, 7);

//** let's generate pairs of chosen-plaintext
generate_pairs(4);

//** pick a pair which meet the characteristic
identifying_good_pair(7);

//** find the key
brute_cracking();
}
```

# Linear Cryptanalysis

Linear cryptanalysis was developed as a theoretical framework for the DES (data encryption system) and was implemented in 1993. Linear cryptanalysis is commonly used inside block ciphers and is a very good starting point for designing and executing complex attacks.

Linear cryptanalysis is defined as a linear relationship that is set between the key, the plaintext structure, and the ciphertext structure. The plaintext is structured and represented as characters or bits. It is required to have a structure of a chain of operations characterized by exclusive-or, as we describe here,

$A_{i_1} \oplus A_{i_2} \dots \oplus A_{i_u} \oplus B_{j_1} \oplus B_{j_2} \oplus \dots \oplus B_{j_v} = Key_{k_1} \oplus Key_{k_2} \oplus \dots Key_{k_w}$

where ⊕ represents the XOR operation as a binary operation, $A_i$ represents the bit from $i^{th}$ position of input the structure $A = [A_1, A_2, ...]$, $B_j$ represents the bit from $j^{th}$ position of the output structure $B = [B_1, B_2, ...]$, and $Key_k$ represents the $k^{th}$ bit of the key $Key = [Key_1, Key_2, ...]$.

# Performing Linear Cryptanalysis

Usually, in the most important cases, performing a linear cryptanalysis starts from the idea that we are aware and we acknowledge the encryption algorithm except the private key. Executing a linear cryptanalysis against a block cipher is represented as a framework described here: