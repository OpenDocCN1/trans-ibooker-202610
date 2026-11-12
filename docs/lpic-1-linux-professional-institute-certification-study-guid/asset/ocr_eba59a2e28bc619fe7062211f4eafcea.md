When you issue the preceding command, it will ask for the passphrase that identifies your private key. A snipped example of this process is shown in Listing 10.46.

# Listing 10.46: Decrypting a file using the recipient's private (secret) key

```
$ ls
encryptfile  examples.desktop
$
$ whoami
rich
$
$ gpg --out CBmessage.txt --decrypt encryptfile
[...]
gpg: encrypted with 3072-bit RSA key, ID E3E4D21F82775FA1, created 2019-06-14
      "Richard Blum <rb4242@gmail.com>"
$
$ ls CBmessage.txt
CBmessage.txt
$
$ cat CBmessage.txt
Rich,
I am almost done with Chapter 10.
What shall we write about next?
The number 1138?
$
```

> We find that people who are new to public/private (asymmetric) key encryption get confused concerning when to use their public key and when to use their private one. If someone wants to send an encrypted file to you, think, “it’s all about me.” They will be using your public key to encrypt the file, and you’ll be using your private key to decrypt it. (Of course, it’s all about the other person when you want to send them an encrypted file.)

# Signing Messages and Verifying Signatures

While encrypting helps to protect the privacy of a document, it does not protect the document from being modified in transit. Using the example back in Figure 10.1, if a malicious actor (Evelyn) gets ahold of Helen’s public key, the threat actor could encrypt a message for Helen and claim it came from Bob. Or worse, if Bob is sending Helen source code, a threat actor could modify it in some way before it reaches Helen.

Fortunately, you can digitally sign gpg encrypted files. This process creates a time stamp and certifies the file. Thus, if the file is modified in any way, the gpg utility will alert the file’s receiver when checked.