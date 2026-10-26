```
010000001712e5b4e97ab549d50ca60a4f5968b2225215e9fab82dae4720078711406972f000000
017160014848202fc47fb475289652fbd1912cc853ecb0096feffffff023236000000000001976a9
14121ae7a2d55d2f0102ccc117cbcb70041b0e037f88ac10270000000000001976a914ec0be509516
51261765cfa71d7bd41c7b9245bb388ac075a0700

- 01000000 - version
- 01 - # of inputs
- 712e...2f - previous tx hash
- 00000000 - previous tx index
- 1716...96 - ScriptSig
- feffffff - sequence
- 02 - # of outputs
- 3236...00 - output amounts
- 1976...ac - ScriptPubKey
- 075a0700 - locktime
```

Figure 13-9. Pay-to-script-hash-pay-to-witness-pubkey-hash (p2sh-p2wpkh) to pre-BIP0141 software

```
01000000000101712e5b4e97ab549d50ca60a4f5968b2225215e9fab82dae4720078711406972f000
0000017160014848202fc47fb475289652fbd1912cc853ecb0096feffffff0232360000000000019
76a914121ae7a2d55d2f0102ccc117cbcb70041b0e037f88ac10270000000000001976a914ec0be50
951651261765cfa71d7bd41c7b9245bb388ac024830450221009263c7de80c297d5b21aba846cf6f0
a970e1d339568167d1e4c1355c7711bc1602202c9312b8d32fd9c7acc54c46cab50eb7255ce3c0122
14c41fe1ad91bcb16a13012102ebdf6fc448431a2bd6380f912a0fa6ca291ca3340e79b6f0c1fdaf
f73cf54061075a0700

- 01000000 - version
- 00 - Segwit marker
- 01 - Segwit flag
- 01 - # of inputs
- 712e...2f - previous tx hash
- 00000000 - previous tx index
- 1716...96 - ScriptSig
- feffffff - sequence
- 02 - # of outputs
- 3236...00 - output amounts
- 1976...ac - ScriptPubKey
- 0248...61 - witness
- 075a0700 - locktime
```

Figure 13-10. p2sh-p2wpkh to post-BIP0141 software

The difference from p2wpkh is that the ScriptSig is no longer empty. The ScriptSig has a RedeemScript, which is equal to the ScriptPubkey in p2wpkh. As this is a p2sh script, the ScriptPubKey is the same as in any other p2sh script. The combined script looks like Figure 13-11.