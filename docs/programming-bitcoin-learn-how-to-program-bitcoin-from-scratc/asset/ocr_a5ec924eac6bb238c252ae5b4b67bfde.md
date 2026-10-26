![](bbox=[0.13, 0.08, 0.84, 0.19])

Figure 8-14. p2sh pattern that executes the special rule

Upon execution of this sequence, if the stack is left with a 1, the RedeemScript is inserted into the Script command set. In other words, if we reveal a RedeemScript whose hash160 is the same as the hash160 in the ScriptPubKey, that RedeemScript acts like the ScriptPubKey instead. We hash the script that locks the funds and put that into the blockchain instead of the script itself. This is why we call this ScriptPubKey pay-to-script-hash.

Let’s go through exactly how this works. We start with the Script commands (Figure 8-15).

![](bbox=[0.13, 0.4, 0.84, 0.6])

Figure 8-15. p2sh start

OP_0 will push a 0 to the stack, and the two signatures and the RedeemScript will be pushed to the stack directly, leading to Figure 8-16.