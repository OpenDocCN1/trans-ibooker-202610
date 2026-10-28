Protocols implementing zero-knowledge proofs of knowledge require input from the verifying party. This input is usually provided in the form of a set of challenges/questions. These responses will convince the verifier if, and only if, a statement is true. Zero-knowledge proofs were first formulated by Shafi Goldwasser, Silvio Micali, and Charles Rackoff in their 1989 paper “The Knowledge Complexity of Interactive Proof-Systems.”

There is even a type of zero-knowledge proofs in which no interaction is necessary between the two parties; there are known as non-interactive zero-knowledge proofs (NIZK). Back in 1988 Blum et al. introduced the idea that a small random string of bits shared by the prover and the verifier guarantees computational zero-knowledge without any interaction between the parties (Blum, Feldman, & Micali, 1988).

# Commitment Schemes

Cryptographic primitives known as commitment schemes are often used in tandem with the approach of zero-knowledge proofs. You can think of a commitment scheme as firmly locked safe which is shipped off to the receiver. It can only be opened if/when the sender gives the combination to the receiver. Once locked, the message cannot be altered even by the sender.

Commitment scheme operation takes place in two phases. First, in the commit phase values are chosen. In the following reveal phase, the value is both revealed and verified. Perhaps the most classic example of a commitment scheme is known as coin flipping by telephone. For this we’ll ask our old friends Alice and Bob to demonstrate.

+   1. Alice commits to a random bit bₐ (“flips a coin”) and sends the resulting commitment C to Bob. Think of C as being a securely locked safe. Alice can’t change the information in this safe/commitment after sealing it and shipping it off.