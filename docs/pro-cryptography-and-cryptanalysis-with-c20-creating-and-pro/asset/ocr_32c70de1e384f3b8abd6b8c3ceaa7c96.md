+   - **General computations between two entities** (two-party computations): Consider two parties A and B. Each of them owns a secret input, x, and y, respectively, and a common function F known by both. To apply the function F over its private input x, the party A computes r = F(x, y). From here, A learns only the value of r and learns nothing about y. On the other hand, B learns nothing about x or r. This is the same as B computing Fᵧ(x) in the semi-honest model, where A encrypts x and sends to B because the semantic security assures the fact that B will learn nothing about the plain value corresponding to x. In such situations, using FHE would simplify things, because A would just apply F as F(x, y), and achieve the result in an encrypted format, but it would need and learn nothing else because everything is encrypted, including F.

# Practical Example of Using FHE

There are more libraries for C++ that implement fully homomorphic encryption. Some well-known libraries for C++ FHE are the following:

+   - HElib [12], developed at IBM, implements the schemes BFV (Brakerski/Fan-Vercauteren) [17] and CKKS (Cheon-Kim-Kim-Song) [18] and it can be used in Linux and MacOS distributions.
+   - TFHE [13] implements the scheme proposed in [15] and it can be used with Linux distributions. In the same paper, the library is described.
+   - PALISADE [14] implements the BGV (Brakerski-Gentry-Vaikuntanathan) [16], BFV [17], CKKS [18], and FHEW schemes [19] and a more secure version of the TFHE scheme [13], including bootstrapping. It is supported on Linux, Windows, and macOS distributions.
+   - SEAL [20]-[23] implements the BFV [17] and CKKS [18] schemes and it can be used with .Net or C++. In addition, the SEAL library can be used in Windows, Linux, or MacOS environments.