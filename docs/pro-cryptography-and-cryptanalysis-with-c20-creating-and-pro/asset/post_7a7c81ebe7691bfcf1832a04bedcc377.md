
Let’s assume that we have a number situated between 1 and 16. We can see that is very easy to find the image of it under f. Without having the table in front of you, for example, for 7 it is hard to find a given that f(a) = 7. If the number that you are given is 3, then is quite easy to see that a = 1 is what you actually need.

Remember that this example is focused on very small numbers. The key thing here is that the amount of effort to measure f(a) is different than the amount of work in finding a given f(a). Also for large numbers, f(a) can be efficiently computed using the square-and-multiply algorithm [20], where the process of finding a from f(a) is harder to find.

Example 2.15 [18]. (one-way function) A prime number is defined as a positive integer. The integer is bigger than 1 and its positive integers divisors are 1 and itself. Let’s take into consideration primes p = 50633 and q = 58411, compute n = pq = 50633 · 58411 = 2957524163, and consider A = {1, 2, 3, …, n − 1}. We will define a function f on A by f(a) = r_a for each a ∈ A, where r_a represents the remainder when x³ is divided by n. For example, let’s consider f(2489991) = 1981394214 since 2489991³ = 5881949859 · n + 1981394214. Computing f(a) represents a simple thing to be done, but reversing the procedure is quite difficult.

## Trapdoor One-Way Functions

Definitions 2.16 [18]. A trapdoor one-way function is represented as a one-way function f: A → B with an extra property that by having information (also known as trapdoor information) it will be much more feasible to have an identification for any given b ∈ Im(f), with an a ∈ A in such way that f(a) = b.

Example 2.15 shows the concept of a trapdoor one-way function. With extra information about the factors of n = 2957524163 it becomes much easier to invert the function. The factors of 2957524163 are large enough that it would be difficult to identify them by hand calculation. We should be able to identify the factors very easily with the help of a computer program. For example, if we have very big, distinct prime numbers (each number has about 200 decimal digits) p and q, with the technology of today, finding p and q from n is very difficult even with the most powerful quantum computers. This is the well-known factorization problem known as integer factorization problem.

One-way and one-way trapdoor functions form the fundamental basis for public-key cryptography. These principles are very important, and they will become much clearer later when you explore the implementation of cryptographic techniques. It is vital and important to understand these concepts from this section because they are the main methods and the primary foundation for the cryptography algorithms implemented later in this chapter.

## Permutations

Permutation represents functions that are in cryptographic constructs.

Definition 2.17 [18]. Consider S to be a finite set formed of elements. A permutation p on S represents a bijection as defined in Definition 2.8. The bijection is represented from S to itself, p : S → S.

Example 2.18 [18]. This example represents a permutation example. Let’s consider the following permutation: S = {1, 2, 3, 4, 5}. The permutation p : S → S is defined as

p(1) = 2, p(2) = 5, p(3) = 4, p(4) = 2, p(5) = 1

A permutation can be described in different ways. It can be written as above or as an array as in

p = (1 2 3 4 5; 3 5 4 2 1),

in which the top row in the array is represented by the domain and the bottom row is represented by the image under p as mapping.

Since the permutations are bijections, they have inverses. If the permutation is written as an array (second form), its inverse can be easily found by interchanging the rows in the array and reordering the elements from the new top row and the bottom row accordingly. In this case, the inverse of p is defined as follows:

p⁻¹ = (1 2 3 4 5; 5 4 1 3 2)

Example 2.19 [18]. This example represents a permutation example. Let’s consider A to be the set of integers {0, 1, 2, ..., p · q − 1} where p and q represent two distinct large primes. We need to suppose also that neither p − 1 nor q − 1 can be divisible by 3. The function p(a) = rₐ, in which rₐ represents the remainder when a³ is divided by p q, can be demonstrated and shown as being the inverse permutation. The inverse permutation is computationally infeasible by the computers of today, unless p and q are known.

## Involutions

Involutions are known as functions having their own inverses.

**Definition 2.20 [18].** Let’s consider a finite set \(S\) and \(f\) defined as a bijection \(S\) to \(S\), noted as \(f:S\to S\). In this case, the function \(f\) will be noted as an *involution* if \(f=f^{-1}\). Another way of defining this is \(f(f(a))=a\) for any \(a\in S\).

**Example 2.21 [18].** This example represents an involution case. Figure 2-4 shows an example of involution. Note that if \(j\) represents the image of \(i\), then \(i\) represents the image of \(j\).

![](img/c3d9424f64735276d661e7aba725b2a2_42_0.png)

## Concepts and Basic Terminology

It is very difficult to understand how cryptography was built using hard and abstract definitions when dealing with the scientific side of the field. In the following sections, we will list the most important terms and key concepts used in this chapter.

## Domains and Codomains Used for Encryption

- \(\mathcal{A}\) is shown as a finite set known as the *alphabet of definition*. We will consider as an example \(\mathcal{A}=\{0,1\}\), which represents the binary alphabet, an alphabet frequently used as definition.
- \(\mathcal{M}\) is a set known as the *message space*. The message space has the strings of symbols from an alphabet, \(\mathcal{A}\). As an example, \(\mathcal{M}\) may have binary strings, English text, French text, etc.

![](img/c3d9424f64735276d661e7aba725b2a2_45_0.png)

# Figure 2-5. Example of two-party communication process applying encryption

## Digital Signatures

In this book, another technique that we will work with is the digital signature. Digital signatures are very important in some processes, like authentication, authorization, or non-repudiation. The digital signature is used to map an individual’s identity with a piece of information. When “something” is digitally signed, it means that the message and the confidential information owned by an individual are converted into a tag called a signature.

The components of the signing process are

- ℳ is the set of messages that can be signed.
- 𝒮 is the set of signatures. They can have a form of binary strings with a predefined length.
- 𝒮_A represents the transformation between ℳ and 𝒮, called a signing transformation, and it is made by entity A. The entity will keep 𝒮_A secret and will use it to sign messages from ℳ.
- V_A is the transformation between ℳ×𝒮 to the set {true,false}. The Cartesian product ℳ×𝒮 contains the pair of elements (m,s) where m∈ℳ and s∈𝒮. The transformation V_A is public and it is used by different entities to check if the signatures were created by entity A.

### Signing Process

Entity A, called the signer, creates a signature s ∈ S for a particular message m ∈ M following these steps:

- Compute s = S_A(m).
- Transmit the pair (m, s) to the desired receiver.

### Verification Process

When the receiver entity B wants to check if entity A created the signature s for the message m, it proceeds as follows:

- Obtain the verification function V_A for the entity A.
- Compute u = V_A(m, s).
- If u = true, then the signature was created by entity A; if u = false, then the signature was not created by entity A.

## Public-Key Cryptography

Public-key cryptography (PKC) has an important role in C++ when similar algorithms need to be incorporated. Many significant commercial libraries implement developer-specific public-key cryptography solutions, such as [21-30].

Next, you will see how the public-key cryptography works. For this, recall that K is the key space. Let’s consider the set of the encryption transformations as {E_e : e ∈ K} and the set of the decryption transformations as {D_d : d ∈ K}. Further, let’s consider the pair of encryption and decryption transformations as (E_e, D_d), where E_e can be learned by anyone, for every e. From having E_e, determining D_d must be computationally unrealizable; in other words, from a random ciphertext c ∈ C it’s impossible to find out the message m ∈ M, such that E_e(m) = c. This property is strong and it means that the corresponding decryption key d (which must be secret/private) may not be computed/determined from either given e (which is public).

With the settings from above, take a look at Figure 2-6 and let’s consider the communication channel between two parties, namely Alice and Bob.
