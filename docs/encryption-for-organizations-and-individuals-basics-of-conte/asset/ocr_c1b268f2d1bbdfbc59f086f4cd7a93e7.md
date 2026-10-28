# Related Key Attack

Whenever a malicious actor can observe multiple keys/passwords being used in a cryptographic context, he or she may be eventually able to decrypt a secure system. This is done by discovering mathematical consistencies between the data in the keys and is called a *related key attack*. This is mostly a theoretical threat especially against newer encryption algorithms like the AES.

**Best countermeasure Against Related Key Attacks:**

+   - Using random-key generation approaches, such as a *key derivation function (KDF)*. A KDF derives multiple keys, whenever desired, from a single *master key* using a pseudorandom function.

In cryptography, a *pseudorandom function (PRF)* refers to one of the most robust means of generating random numbers. It's not the same as a *pseudorandom generator (PRG)*. A PRG offers a **single output** at random if the **input** was chosen at random. Now, with a PRF **all** of its outputs seem random regardless of how the corresponding inputs were chosen.

# Rubber-Hose Attack

Perhaps the crudest type of cryptanalysis, *rubber-hose attacks* refer to subjecting the owner of a key/password to coercion or physical violence to extract said information. Cryptography isn't always diplomatic. We'll be looking at legal aspects and the rights of password holders in Chapter 7.

**Countermeasures Against Rubber-Hose Attacks:**

+   - Staying out of countries with poor human rights records
+   - Choosing your friends carefully