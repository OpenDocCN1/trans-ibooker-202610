
*denoted with Eval, which describes formally the most important rule mentioned above. The input and the output of the Eval algorithm are in an encrypted format. In the Eval algorithm, the function g is applied over encrypted data c₁ and c₂, without accessing the plain data m₁ and m₂, and having the following property:*

Dec(key_priv, Eval_g(key_eval, c₁, c₂)) = f(m₁, m₂)

In homomorphic encryption, only two operations are required to have homomorphic properties, namely addition and multiplication. This is due to the fact that an arbitrary function can be represented as a circuit using just gates corresponding to the addition operation (OR gate) and the multiplication operation (AND gate). The idea of homomorphic encryption started in the late 70s, when it was called *privacy homomorphism* [1]. Among the first encryption schemes to have homomorphic properties is the Unpadded RSA algorithm [2], in which the operation with homomorphic properties is the multiplication:

Encryption(m₁) Encryption(m₂) = m₁^e m₂^e mod n
= (m₁ m₂)^e mod n
= Encryption(m₁ · m₂)

In the above computation, m₁, m₂ are two plain messages and *Encryption* is the encryption function.

The homomorphic encryption schemes can be categorized into three classes, as follows:

- **Partial homomorphic encryption (PHE)**: The schemes in this category support just one operation applied over encrypted data an unlimited number of times. Examples of PHE schemes are RSA [2], Goldwasser-Micali [3], and El-Gamal [4]. Most of the schemes from this category represent a basis for other homomorphic schemes.
- **Somewhat homomorphic encryption (SWHE)**: The schemes in this category support both operations applied on the encrypted data, but for a limited number of times. The encryption scheme from [5] is an example of SWHE.
- **Fully homomorphic encryption (FHE)**: The schemes in this category support both operations over encrypted data for an unlimited number of times. Fully homomorphic encryption is considered “cryptography’s holy grail” or “the Swiss Army knife of cryptography” [6] due to its capability of enabling any computation over the encrypted data by any number of times. In 2009, the first FHE scheme [7] was proposed and the mathematical object used as the foundation is the ideal lattices. The scheme from [7] is very important in cryptography because it opened the way for the FHE schemes, and even though it is unpractical in the form in which it was proposed due to its complexity and abstraction, it represented a basis for subsequent schemes. In addition, in [7] a general framework for the FHE schemes was proposed.

## Fully Homomorphic Encryption

In this section, fully homomorphic encryption (FHE) is explained in more detail because it represents an important topic of cryptography that can resolve many security concerns and issues. A particular model of quantum computation called boson scattering enables a quantum homomorphic encryption that provides theoretically limited security.

By existing, this kind of scheme makes us wonder if quantum methods can generate theoretically secure FHE schemes. In [25] the authors prove that quantum techniques do not enable efficient theoretically secure FHE that hides completely the plaintext.

As mentioned in the previous section, the first FHE scheme was proposed by Craig Gentry in 2009 and the mathematical object that represents the foundation is the ideal lattices with the hardness assumption (problems regarding a topic that cannot be solved in an efficient time, i.e in polynomial time) called the ideal coset problem. Following Gentry’s scheme, there were proposed a large number of FHE schemes based on different mathematical techniques. A subsequent work is [8], in which the FHE scheme uses integer arithmetic. However, the noise introduced in the schemes from [7] and [8] grows quickly, representing a drawback because it has a high effect over the applicability and security, thus the homomorphic capabilities are restricted. Due to the noise growth, the decryption cannot be made after some point.

In the second generation of the FHE schemes that include works such as [9] and [10], the noise is handled more efficiently, which means improved performance and powerful

security under various hardness assumptions. The *leveled encryption schemes* and *bootstrappable encryption schemes* are results of this generation. The first ones evaluate the circuits with a given polynomial depth, while the second ones can be modified to become FHE schemes. If an encryption scheme has the capability of evaluating its decryption circuit and additionally one NAND gate, then it is a bootstrappable encryption scheme.

The third generation of FHE schemes is opened by the work of [11], which uses a new technique to handle the noise. The schemes of the third generation are less performant than those from the second generation, but their hardness assumptions can be weaker. The basis for many schemes in this generation is asymmetric multiplication. That is, considering two encrypted texts c₁, c₂, the product c₁ • c₂ is different from the product c₂ • c₁, although both products encrypt the same product b₁ • b₂ of the plain texts b₁ and b₂.

FHE can be used in many areas of cryptography, such as

- **Outsourcing:** The private data can be kept safe if it is stored in third-party storage or analyzed by third-party entities. A classic example for this area is that of a company that stores its data in cloud storage. Before uploading the data in the cloud, the owner needs to encrypt it. FHE would be useful in such scenarios because the cloud provider could analyze the data from the company in an encrypted format, without accessing the plain data. Moreover, the result of the computations would be sent by the cloud provider in the encrypted format to the data owner, where it would be decrypted only by the decryption key’s owner.
- **Private information retrieval (PIR) or private queries:** PIR and private queries are useful when a database is queried or an application uses a search engine. Another scenario is when a client wants to send a query to a database server, but the client wants the server to learn nothing about its query. The solution is the following: the client encrypts the query and sends it to the server and then the server applies the encrypted query over encrypted data and responds with the encrypted result.
- **General computations between two entities** (two-party computations): Consider two parties A and B. Each of them owns a secret input, x, and y, respectively, and a common function F known by both. To apply the function F over its private input x, the party A computes r = F(x, y). From here, A learns only the value of r and learns nothing about y. On the other hand, B learns nothing about x or r. This is the same as B computing Fᵧ(x) in the semi-honest model, where A encrypts x and sends to B because the semantic security assures the fact that B will learn nothing about the plain value corresponding to x. In such situations, using FHE would simplify things, because A would just apply F as F(x, y), and achieve the result in an encrypted format, but it would need and learn nothing else because everything is encrypted, including F.

## Practical Example of Using FHE

There are more libraries for C++ that implement fully homomorphic encryption. Some well-known libraries for C++ FHE are the following:

- HElib [12], developed at IBM, implements the schemes BFV (Brakerski/Fan-Vercauteren) [17] and CKKS (Cheon-Kim-Kim-Song) [18] and it can be used in Linux and MacOS distributions.
- TFHE [13] implements the scheme proposed in [15] and it can be used with Linux distributions. In the same paper, the library is described.
- PALISADE [14] implements the BGV (Brakerski-Gentry-Vaikuntanathan) [16], BFV [17], CKKS [18], and FHEW schemes [19] and a more secure version of the TFHE scheme [13], including bootstrapping. It is supported on Linux, Windows, and macOS distributions.
- SEAL [20]-[23] implements the BFV [17] and CKKS [18] schemes and it can be used with .Net or C++. In addition, the SEAL library can be used in Windows, Linux, or MacOS environments.
