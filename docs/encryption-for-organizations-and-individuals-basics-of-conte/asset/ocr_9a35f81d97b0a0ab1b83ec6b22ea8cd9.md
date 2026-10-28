The somewhat elderly RSA encryption method, mentioned in the previous chapters, actually represents *partially homomorphic encryption (PHE)*.

Now, there are basically three degrees to homomorphism in cryptography, each with different pros, cons, and usage scenarios. These are summarized in Table 14-1.

# Table 14-1. The three main degrees of homomorphic encryption

| Classification | Features | Potential Usage Scenarios |
|---|---|---|
| Partially homomorphic (PHE) | Permits either addition or multiplication on encrypted data but not both, fast computational speed | Statistical analysis |
| Somewhat homomorphic (SHE) | Supports more than a single type of operation, limits total number of operations | Secure databases |
| Fully homomorphic (FHE) | Arbitrary computation on encrypted data, slowest rate of computation | Cloud-based storage, electronic voting, managing highly classified data |