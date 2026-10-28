# Counter Mode (CTR)

The counter mode is a fully parallelizable stream cipher. This basically means a block-based cipher (e.g., AES) being used as a stream cipher. CTR offers random access processing during both encryption and decryption, which is beneficial in scenarios such as disk drive encryption. Also, this mode, when implemented properly, can leverage the many varieties of special processing instructions found in most modern CPUs to further boost its operating efficiency (e.g., technologies like MMX and SSE).

# In Closing

After finishing this chapter, you will have hopefully gained awareness of the following:

+   - What the Advanced Encryption Standard (AES) is and when it’s used
+   - What the binary and hexadecimal systems are and how to convert between them
+   - How the substitution-permutation network (SPN) in AES works
+   - What steps are a part of a single round of operation in AES
+   - What hashing means and what some of the most popular algorithms for this are
+   - What salting and peppering refer to in the context of cryptography

In the next chapter we’ll go deep into the world of encryption software, including methods included in popular operating systems as well as some of the most relevant third-party products.