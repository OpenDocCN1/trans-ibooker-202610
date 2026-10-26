# Asymmetric and Symmetric Encryption

There are two main types of encryption that use certificates: asymmetric and symmetric. We need to learn about each thoroughly. Let's start by understanding what encryption is, please remember that you are taking plaintext and changing it into ciphertext so that the information cannot be read by outside agencies.

# Encryption Explained

Encryption is where we take plaintext that can be easily read and convert it into ciphertext that cannot be easily read, let us look at some different types of encryption.

+   - **Substitution Cipher**: Julius Caesar, who died in 44 BC, invented the first substitution cipher, where he moved each letter of the alphabet three places one way or another. This way, he could make his military plans unreadable if they had been intercepted. What he forgot about was that most people in those days could not read! This was called ROT 13, after the thirteen-letter rotation, and is now known as the Caesar cipher. For example, if you take the word ECHO and move each letter thirteen places to the right in the alphabet sequence, you will get the word RPUB—that would be difficult for someone to read. To decrypt, you would roll back 13 spaces.

+   - **ROT 13**: ROT 13 is a variation of the Caesar cipher. As there are 26 letters in the alphabet, we are rotating the letters 13 times. The key to ROT 13 would be as follows:

| Letter | A | B | C | D | E | F | G | H | I | J | K | L | M |
| ROT 13 | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
|        |   |   |   |   |   |   |   |   |   |   |   |   |   |
| Letter | N | O | P | Q | R | S | T | U | V | W | X | Y | Z |
| ROT 13 | A | B | C | D | E | F | G | H | I | J | K | L | M |

Figure 2.7 – Caesar Cipher ROT 13 table

When receiving the message, GVZR SBE GRN, then we would apply ROT 13, but instead of going forward 13 places to decipher, we would simply go back 13 places, and the message would be TIME FOR TEA. From the preceding table, select a letter from the top and then the corresponding ROT 13 equivalent below it for both encryption and decryption.