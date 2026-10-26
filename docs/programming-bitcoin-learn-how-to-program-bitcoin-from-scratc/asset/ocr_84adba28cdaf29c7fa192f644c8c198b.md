❷ We make sure that each input has a correct ScriptSig.

Note that a full node would verify more things, like checking for double-spends and checking some other consensus rules not discussed in this chapter (max sigops, size of ScriptSig, etc.), but this is good enough for our library.

# Creating Transactions

The code to verify transactions will help quite a bit with creating transactions. We can create transactions that fit the verification process. Transactions we create will require the sum of the inputs to be greater than or equal to the sum of the outputs. Similarly, transactions we create will require a ScriptSig that, when combined with the ScriptPubKey, will be valid.

To create a transaction, we need at least one output we’ve received. That is, we need an output from the UTXO set whose ScriptPubKey we can unlock. The vast majority of the time, we need one or more private keys corresponding to the public keys that are hashed in the ScriptPubKey.

The rest of this chapter will be concerned with creating a transaction whose inputs are locked by p2pkh ScriptPubKeys.

# Constructing the Transaction

The construction of a transaction requires answering some basic questions:

+   1. Where do we want the bitcoins to go?
+   2. What UTXOs can we spend?
+   3. How quickly do we want this transaction to get into the blockchain?

We’ll be using testnet for this example, though this can easily be applied to mainnet.

The first question is about how much we want to pay whom. We can pay one or more addresses. In this example, we will pay 0.1 testnet bitcoins (tBTC) to mnrVtF8DWjMu839VW3rBfgYaAfKk8983Xf.

The second question is about what’s in our wallet. What do we have available to spend? In this example, we have an output denoted by a transaction ID and output index:

```
0d6fe5213c0b3291f208cba8bfb59b7476dffacc4e5cb66f6eb20a080843a299:13
```