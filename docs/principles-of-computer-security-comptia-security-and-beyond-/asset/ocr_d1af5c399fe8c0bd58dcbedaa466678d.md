first is only needed if you aren’t currently in **nslookup** or if you are in **nslookup** but the previous query was for a resource record other than TXT):

```
set q=txt
rit1608._domainkey.rit.edu.
```

The second command will vary based on the e-mail you chose. The first part, before _domainkey, is the selector specified in the s= value. Selectors are really obscure, and even if they’re short, they’re just not guessable. Then, _domainkey is required, followed by the domain specified in the d= value.

Here’s the output I got from the second **nslookup** command:

```
"v=DKIM1;
p=MIGfMA0GCSqGSIb3DQEB AQUAA4GNADCBiQKBgQDXq97I7+PXPn39GE0fTg3Ypn3Gmtr
A2LsQCNj+q2iyK0Cewa1H9RiM7bF7SbrDFzFF2iQv1PPcHfBpBihpnnnPx
r5txK+UZDEb5b9VDyUr+MC
Wsk7l4w8Fr6DA8Bwn4X5Ykzp6rvJe0AFofG8d/0dhxQyA/1Rg23aXE6ee6
SQjFQIDAQAB;"
```

The fields and their values are as follows:

+   - **v=** The DKIM version (always 1)
+   - **p=** The public key

The receiving e-mail server uses that public key to decrypt the encrypted hash. After that, the receiving mail server computes the hash itself. If the computed hash matches the decrypted hash, the message is allowed to continue on its journey. This is because the encrypted hash, if it decrypts with the sender’s public key, could only have been encrypted with the sender’s private key, and the sender is the only one in possession of this key.

The official DKIM standard recommends switching DKIM keys and revoking old ones at least every six months, leaving a little time of an overlap between old and new.

![](bbox=[0.12, 0.83, 0.25, 0.85])

# **Step 4** DMARC is the third e-mail security mechanism, and it actually