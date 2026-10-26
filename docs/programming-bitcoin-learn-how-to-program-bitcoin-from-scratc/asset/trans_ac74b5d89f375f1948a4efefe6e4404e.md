

This means to provide a correct u and v, we either have to break the discrete log problem or know the secret e. Since we assume discrete log is hard, we can say e is assumed to be known by the one who came up with u and v.

One subtle thing that we haven’t talked about is that we have to incorporate the purpose of our shooting. This is a contract that gets fulfilled as a result of shooting at the target. William Tell, for example, was shooting so that he could save his son (shoot the target and you get to save your son). You can imagine there would be other reasons to hit the target and other “rewards” that the person hitting the target would receive. This has to be incorporated into our equations.

In signature/verification parlance, this is called the signature hash. A hash is a deterministic function that takes arbitrary data into data of fixed size. This is a fingerprint of the message containing the intent of the shooter, which anyone verifying the message already knows. We denote this with the letter z. This is incorporated into our uG + vP calculation this way:

```
u = z/s, v = r/s
```

Since r is used in the calculation of v, we now have the tip of the arrow inscribed. We also have the intent of the shooter incorporated into u, so both the reason for shooting and the target that is being aimed at are now part of the equation.

To make the equation work, we can calculate s:

```
uG + vP = R = kG
uG + veG = kG
u + ve = k
z/s + re/s = k
(z + re)/s = k
s = (z + re)/k
```

This is the basis of the signature algorithm, and the two numbers in a signature are r and s.

Verification is straightforward:

```
uG + vP where u,v ≠ 0
uG + vP = (z/s)G + (re/s)G = ((z + re)/s)G = ((z + re)/((z + re)/k))G = kG = (r,y)
```

uG + vP = (z/s)G + (r/s)P = (z/s)G + (re/s)G = ((z + re)/s)G

We know s = (z + re)/k, so:

uG + vP = ((z + re) / ((z + re)/k))G = kG = R

We've successfully chosen u and v in such a way as to generate R as we intended. Furthermore, we used r in the calculation of v, proving we knew what R would be. The only way we can know the details of R beforehand is if we know e.

To wit, here are the steps:

+   1. We are given (r,s) as the signature, z as the hash of the thing being signed, and P as the public key (or public point) of the signer.
+   2. We calculate u = z/s, v = r/s.
+   3. We calculate uG + vP = R.
+   4. If R's x coordinate equals r, the signature is valid.

![](img/6c915536bcfd43e412f7adef942edf23_91_0.png)

## Why Two Rounds of sha256?

The calculation of z requires two rounds of sha256, or hash256. You may be wondering why there are two rounds when only one is necessary to get a 256-bit number. The reason is for security.

There is a well-known hash collision attack on SHA-1 called a birthday attack that makes finding collisions much easier. Google found a SHA-1 collision using some modifications of a birthday attack and a lot of other things in 2017. Using SHA-1 twice, or double SHA-1, is the way to defeat or slow down some forms of this attack.

Two rounds of sha256 don't necessarily prevent all possible attacks, but doing two rounds is a defense against some potential weaknesses.

## Verifying a Signature

We can now verify a signature using some of the primitives that we have:

```
>>> from ecc import S256Point, G, N
>>> z = 0xbc62d4b80d9e36da29c16c5d4d9f11731f36052c72401a76c23c0fb5a9b74423
>>> r = 0x37206a0610995c58074999cb9767b87af4c4978db68c06e8e6e81d282047a7c6
>>> s = 0x8ca63759c1157ebeaec0d03cecca119fc9a75bf8e6d0fa65c841c8e2738cdaec
>>> px = 0x04519fac3d910ca7e7138f7013706f619fa8f033e6ec6e09370ea38cee6a7574
>>> py = 0x82b51eab8c27c6e2c658a079bcd f4f1ada34cec420cafc7eac1a42216fb6c4
>>> point = S256Point(px, py)
>>> s_inv = pow(s, N-2, N)
```

```
>>> u = z * s_inv % N
>>> v = r * s_inv % N
>>> print((u*G + v*point).x.num == r)
True
```

+   1. Note that we use Fermat's little theorem for 1/s, since n is prime.
+   2. u = z/s.
+   3. v = r/s.
+   4. uG + vP = (r,y). We need to check that the x coordinate is r.

## Exercise 6

Verify whether these signatures are valid:

```
P = (0x887387e452b8eacc4acfde10d9aaf7f6d9a0f975aabb10d006e4da568744d06c,
    0x61de6d95231cd89026e286df3b6ae4a894a3378e393e93a0f45b666329a0ae34)
```

```
# signature 1
z = 0xec208baa0fc1c19f708a9ca96fdeff3ac3f230bb4a7ba4aede4942ad003c0f60
r = 0xac8d1c87e51d0d441be8b3dd5b05c8795b48875dffe00b7ffcfac23010d3a395
s = 0x68342ceff8935eded102dd876ffd6ba72d6a427a3edb13d26eb0781cb423c4
```

```
# signature 2
z = 0x7c076ff316692a3d7eb3c3bb0f8b1488cf72e1afcd929e29307032997a838a3d
r = 0xefff69ef2b1bd93a66ed5219add4fb51e11a840f404876325a1e8ffe0529a2c
s = 0xc7207fee197d27c618aea621406f6bf5ef6fca38681d82b2f06fddbdce6feab6
```

## Programming Signature Verification

We already have a class S256Point, which is the public point for the private key. We create a Signature class that houses the r and s values:

```
class Signature:
    def __init__(self, r, s):
        self.r = r
        self.s = s
    def __repr__(self):
        return 'Signature({:x},{:x})'.format(self.r, self.s)
```

We will be doing more with this class in Chapter 4.

We can now write the verify method on S256Point based on this:

```
class S256Point(Point):
    ...
    def verify(self, z, sig):
```

```
s_inv = pow(sig.s, N - 2, N) ①
u = z * s_inv % N ②
v = sig.r * s_inv % N ③
total = u * G + v * self ④
return total.x.num == sig.r ⑤
```

+   ① `s_inv (1/s)` is calculated using Fermat’s little theorem on the order of the group, `n`, which is prime.
+   
+   ② `u = z/s`. Note that we can mod by `n` as that’s the order of the group.
+   
+   ③ `v = r/s`. Note that we can mod by `n` as that’s the order of the group.
+   
+   ④ `uG + vP` should be `R`.
+   
+   ⑤ We check that the `x` coordinate is `r`.

So, given a public key that is a point on the secp256k1 curve and a signature hash, `z`, we can verify whether a signature is valid or not.

## Signing in Depth

Given that we know how verification should work, signing is straightforward. The only missing step is figuring out what `k`, and thus `R = kG`, to use. We do this by choosing a random `k`.

The signing procedure is as follows:

+   1. We are given `z` and know `e` such that `eG = P`.
+   2. Choose a random `k`.
+   3. Calculate `R = kG` and `r = x` coordinate of `R`.
+   4. Calculate `s = (z + re)/k`.
+   5. Signature is `(r, s)`.

Note that the public key (pubkey) `P` has to be transmitted to whoever wants to verify it, and `z` must be known by the verifier. We’ll see later that `z` is computed and `P` is sent along with the signature.

## Creating a Signature

We can now create a signature.

## Programming Message Signing

To program message signing, we now create a PrivateKey class, which will house our secret:

```
class PrivateKey:
    def __init__(self, secret):
        self.secret = secret
        self.point = secret * G
    def hex(self):
        return '{:x}'.format(self.secret).zfill(64)
```

① We keep around the public key, self.point, for convenience.

We then create the sign method:

```
from random import randint
...
class PrivateKey:
...
    def sign(self, z):
        k = randint(0, N) ①
        r = (k*G).x.num ②
        k_inv = pow(k, N-2, N) ③
        s = (z + r*self.secret) * k_inv % N ④
        if s > N/2: ⑤
            s = N - s
        return Signature(r, s) ⑥
```

① randint chooses a random integer from [0, n). Please don’t use this function in production, because the random number from this library is not nearly random enough.

② r is the x coordinate of kG.

③ We use Fermat’s little theorem again, and n, which is prime.

④ s = (z + re)/k.

⑤ It turns out that using the low-s value will get nodes to relay our transactions. This is for malleability reasons.

⑥ We return a Signature object from the class defined earlier.

## Importance of a Unique k

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

