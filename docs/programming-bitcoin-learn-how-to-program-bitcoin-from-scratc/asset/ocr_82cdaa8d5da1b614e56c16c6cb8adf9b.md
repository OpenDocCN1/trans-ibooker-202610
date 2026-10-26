```
candidate = int.from_bytes(v, 'big')
if candidate >= 1 and candidate < N:
    return candidate
k = hmac.new(k, v + b'\x00', s256).digest()
v = hmac.new(k, v, s256).digest()
```

① We are using the deterministic k instead of a random one. Everything else about sign remains the same.

② This algorithm returns a candidate that's suitable.

A deterministic k will be unique with very high probability. This is because sha256 is collision-resistant, and no collisions have been found to date.

Another benefit from a testing perspective is that the signature for a given z and the same private key will be the same every time. This makes debugging much easier and unit tests a lot easier to write. In addition, transactions that use deterministic k will create the same transaction every time, as the signature will not change. This makes transactions less malleable (more on that in Chapter 13).

# Conclusion

We’ve covered elliptic curve cryptography and can now prove that we know a secret by signing something. We can also verify that the person with the secret actually signed a message. Even if you don’t read another page in this book, you’ve learned to implement what was once considered “weapons-grade munitions”. This is a major step in your journey and will be essential for the rest of the book!

We now turn to serializing a lot of these structures so that we can store them on disk and send them over the network.