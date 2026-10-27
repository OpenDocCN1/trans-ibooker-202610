Different types of ciphers have been used over the history of encryption. Two common cipher types are substitution ciphers and transposition ciphers.

# Substitution Ciphers

A substitution cipher involves substituting one character for another. For example, you might substitute every letter in the message with a letter that is five characters later in the alphabet. This would mean that every letter e would be converted to the letter j, each a would be converted to f, and so on. Here is an example of the result of this substitution cipher:

```
Plain text:  This is a plain text message
Cipher text: Ymnx nx f uqfn s yjcy rjxxflj
```

Two classic examples of algorithms that use substitution ciphers are the Caesar cipher and ROT13. The Caesar cipher substitutes every character in the message by incrementing the characters by a certain number, such as three. The ROT13 cipher increments the character by 13. The following examples show the results of the Caesar cipher and ROT13:

```
Plain text:  This is a plain text message
Caesar cipher: Wklv lv d sodlq whaw phvvdjh
ROT13 cipher: Ftue ue m bxmuz fqjf yqeemsq
```

# Transposition Ciphers

A transposition cipher involves shifting the characters in the message a certain number of places. For example, in the following example, the message is encrypted by shifting the characters two placeholders to the right:

```
Plain text: Glen
Cipher text: enGl
```

> exam Watch
> For the Security+ exam, remember that a substitution cipher replaces a character with another character, while a transposition cipher shifts the places of the characters.

# Hashing

Hashing is used to maintain the integrity of the information. With hashing, the goal is to prove that the information has not been altered since the data was sent to a recipient or stored on disk. In order to verify that the data has not been changed, it is run through a hashing algorithm to generate a hash value (known as a message digest). The hash value is then stored with the data.

When the data is read later, it is run through the hashing algorithm again to have a new hash value generated. The new hash value is then compared to the stored hash value, and if the two are the same, then the data has not been altered (see Figure 12-2).