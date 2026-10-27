The following summarizes the formula for XOR functions when two bits are being XORed against one another:

```
0+0=0
0+1=1
1+0=1
1+1=0
```

For encryption, the plain text is XORed with the key to create the cipher text. Consider the following example:

```
Plain text: 10110101
Key:        01101011
Cipher text: 11011110
```

After the plain text is encrypted to create the cipher text, the principle behind the XOR function is that you can decrypt the information by XORing the cipher text against the key again:

```
Cipher text: 11011110
Key:         01101011
Plain text:  10110101
```

Now that you understand the basic cryptography terminology, let’s take a look at the different types of encryption.

# Salt, IV, and Nonce

Most encryption implementations use a key, but many also use salt values, initialization vectors, and nonce values to strengthen the encryption or hashing function.

![](bbox=[0.1, 0.6, 0.5, 0.75])

> Watch For the Security+ exam, be familiar with the terms work factor, one-time pad (OTP), and exclusive OR (XOR).

A salt is a random value that is generated and combined with the data before the data is hashed. This is a technique known as salting and is used to ensure that the same data input (typically a password) will generate different hash values when the password hashes are generated.

Similar in concept to a salt, an initialization vector, or IV, is a random value generated that is used with the encryption key to encrypt data. The purpose of joining the IV with the key is to ensure that if a hacker were to gain access to the encrypted data, they could not compare different parts of the encrypted data to try to determine the key value. The IV is giving randomness to each segment of data that is encrypted with the same key.

A nonce is a randomly generated number that is only used once and is typically applied to authentication traffic. The typical scenario for a nonce is at the beginning of the authentication process. The client sends a request to the server for a nonce value. The server sends a random number to the client, which then uses that number to hash the password (with a hashing algorithm) and sends the hashed authentication information across the network to the server.