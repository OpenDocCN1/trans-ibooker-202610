# Segwit

Segwit stands for “segregated witness” and is a backward-compatible upgrade or “soft fork” that activated on the Bitcoin network in August 2017. While the activation was controversial, the features of this technology require some explanation. In this chapter, we’ll explore how Segwit works, why it’s backward compatible, and what Segwit enables.

As a brief overview, Segwit incorporated a multitude of changes:

+   - Block size increase
+   - Transaction malleability fix
+   - Segwit versioning for clear upgrade paths
+   - Quadratic hashing fix
+   - Offline wallet fee calculation security

It’s not entirely obvious what Segwit is without looking at how it’s implemented. We’ll start by examining the most basic type of Segwit transaction, pay-to-witness-pubkey-hash.

# Pay-to-Witness-Pubkey-Hash (p2wpkh)

Pay-to-witness-pubkey-hash (p2wpkh) is one of four types of scripts defined by Segwit in BIP0141 and BIP0143. This is a smart contract that acts a lot like pay-to-pubkey-hash and is named similarly for that reason. The main change from p2pkh is that the data for the ScriptSig is now in the witness field. The rearrangement is to fix transaction malleability.