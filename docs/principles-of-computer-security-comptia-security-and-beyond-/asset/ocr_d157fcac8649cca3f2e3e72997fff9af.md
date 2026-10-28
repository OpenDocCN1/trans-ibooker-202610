Alice both derived, which is now the shared secret/symmetric used for encryption and decryption.

# Step 3 Why did that work?

+   - Bob did 3¹² ≡ 16 mod 29. The 3 he got from Alice was calculated as 3 ≡ 2⁵ mod 29. So Bob really did 2⁵⁽¹²⁾ mod 29.

# ⇒ Note

The congruent symbol (≡) is used instead of the equals symbol with modular arithmetic because there are many numbers when divided by the modulus that will produce the same remainder.

+   - Alice did 7⁵ ≡ 16 mod 29. The 7 she got from Bob was calculated as 7 ≡ 2¹² mod 29. So Alice really did 2⁽¹²⁾⁵ mod 29.

+   - You can see that 2 raised to the power of 5 raised to the power of 12 is the same as 2 raised to the power of 12 raised to the power of 5. In both cases, you multiply 12 by 5, to get 2 raised to the power of 60, and when that’s divided by 29, a remainder of 16 is produced.

# 4

# Step 4 Now it’s your turn! If two sides agree on a prime number of 13 and a generator of 6, what’s the shared secret if one side has a private number of 5 and the other has a private number of 4? You may use any tool, including Google, to do the math.

# 5

# Step 5 Try another one! If two sides agree on a prime number of 11 and a generator of 7, what’s the shared secret if one side has a private number of 3 and the other has a private number of 6? You may use any tool, including Google, to do the math.

# Lab Analysis