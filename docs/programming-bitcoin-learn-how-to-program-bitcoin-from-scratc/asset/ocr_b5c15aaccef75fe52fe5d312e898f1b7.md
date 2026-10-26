# Importance of a Unique k

There’s an important rule in signatures that utilize a random component like we have here: the k needs to be unique per signature. That is, it cannot get reused. In fact, a k that’s reused will result in you revealing your secret! Why?

If our secret is e and we are reusing k to sign z₁ and z₂:

```
kG = (r,y)
s₁ = (z₁ + re) / k, s₂ = (z₂ + re) / k
s₁/s₂ = (z₁ + re) / (z₂ + re)
s₁(z₂ + re) = s₂(z₁ + re)
s₁z₂ + s₁re = s₂z₁ + s₂re
s₁re - s₂re = s₂z₁ - s₁z₂
e = (s₂z₁ - s₁z₂) / (r s₁ - r s₂)
```

If anyone sees both signatures, they can use this formula and find our secret! The PlayStation 3 hack back in 2010 was due to the reuse of the k value in multiple signatures.

To combat this, there is a deterministic k generation standard that uses the secret and z to create a unique, deterministic k every time. The specification is in RFC 6979 and the code changes to look like this:

```
class PrivateKey:
    ...
    def sign(self, z):
        k = self.deterministic_k(z) ①
        r = (k * G).x.num
        k_inv = pow(k, N - 2, N)
        s = (z + r * self.secret) * k_inv % N
        if s > N / 2:
            s = N - s
        return Signature(r, s)

    def deterministic_k(self, z):
        k = b'\x00' * 32
        v = b'\x01' * 32
        if z > N:
            z -= N
        z_bytes = z.to_bytes(32, 'big')
        secret_bytes = self.secret.to_bytes(32, 'big')
        s256 = hashlib.sha256
        k = hmac.new(k, v + b'\x00' + secret_bytes + z_bytes, s256).digest()
        v = hmac.new(k, v, s256).digest()
        k = hmac.new(k, v + b'\x01' + secret_bytes + z_bytes, s256).digest()
        v = hmac.new(k, v, s256).digest()
        while True:
            v = hmac.new(k, v, s256).digest()
```