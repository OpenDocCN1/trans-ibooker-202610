![](bbox=[0.12, 0.15, 0.82, 0.35])

# Figure 2-5. Example of two-party communication process applying encryption

# Digital Signatures

In this book, another technique that we will work with is the digital signature. Digital signatures are very important in some processes, like authentication, authorization, or non-repudiation. The digital signature is used to map an individual’s identity with a piece of information. When “something” is digitally signed, it means that the message and the confidential information owned by an individual are converted into a tag called a signature.

The components of the signing process are

+   - ℳ is the set of messages that can be signed.
+   - 𝒮 is the set of signatures. They can have a form of binary strings with a predefined length.
+   - 𝒮_A represents the transformation between ℳ and 𝒮, called a signing transformation, and it is made by entity A. The entity will keep 𝒮_A secret and will use it to sign messages from ℳ.
+   - V_A is the transformation between ℳ×𝒮 to the set {true,false}. The Cartesian product ℳ×𝒮 contains the pair of elements (m,s) where m∈ℳ and s∈𝒮. The transformation V_A is public and it is used by different entities to check if the signatures were created by entity A.