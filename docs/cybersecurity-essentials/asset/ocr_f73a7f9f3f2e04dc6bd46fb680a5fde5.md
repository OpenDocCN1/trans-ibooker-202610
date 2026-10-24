+   - The encryption keys or secrets are not stored anywhere.
+   - The connection between the client and server is secure and encrypted.

That same system can be made more secure by including more factors and by adding physical factors, such as fingerprint scanning or another biometric authentication system, as described in Chapters 1 and 2. However, while the industry has been producing device solutions for years, mainstream acceptance by users takes a much longer time.

If you use a credential-based access system that does not use good security techniques, then you must understand the risk associated with that activity. If very little is at risk, then just using good password policy might provide enough security.

# Cryptography

*Cryptography* is the term used to describe the concepts and methods for securing information. Many researchers have built their careers on a single cryptographic algorithm. Some of these methods are interesting historically and can be fascinating to read about, but we will deal with only a few of the modern areas of cryptography.

Many cryptographic techniques involved the use of keys. A *key* is merely a data string used to encrypt or decrypt information. How the key is used in this way and how large the string is does impact the strength of the encryption.

Encryption keys can be based on a “secret” string that is known only to the software that encrypts and decrypts the data, or it may be randomly generated. It could also be a combination of known and random factors.

The algorithm that performs this encryption or decryption is known as a *cipher*. A cipher might be a stream cipher dealing with one character at a time or a block cipher that deals with multiple blocks of an input string at one time.

If the same key is used for both encryption and decryption, then it is a *symmetric* key. If a different key is used for encryption than decryption, then these are known as *asymmetric* keys even if the keys are based on one another. The difference is shown in Figure 22.3.

Public-key cryptography uses asymmetric keys incorporating a public key and a private key (or secret key). Public and private keys are different. A person’s public key is widely shared, while their private key must be kept secret to them only. Asymmetric key cryptography when used correctly offers two services: encryption and *authentication* (proving someone is who they say they are).