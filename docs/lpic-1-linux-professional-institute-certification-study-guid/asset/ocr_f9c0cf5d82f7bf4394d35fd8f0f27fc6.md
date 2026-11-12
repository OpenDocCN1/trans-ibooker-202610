# # Generating SSH Keys

Typically, OpenSSH will search for its system's public/private key pairs. If they are not found, OpenSSH automatically generates them. These key pairs, also called *host keys*, are stored in the /etc/ssh/ directory within files. In Listing 10.35, key files' names are displayed.

# **Listing 10.35: Looking at OpenSSH key files**

```
$ ls -1 /etc/ssh/*key*
/etc/ssh/ssh_host_ecdsa_key
/etc/ssh/ssh_host_ecdsa_key.pub
/etc/ssh/ssh_host_ed25519_key
/etc/ssh/ssh_host_ed25519_key.pub
/etc/ssh/ssh_host_rsa_key
/etc/ssh/ssh_host_rsa_key.pub
$
```

In Listing 10.35, both private and public key files are shown. The public key files end in the .pub filename extension, whereas the private keys have no filename extension. The filenames follow this standard:

```
`ssh_host_KeyType_key`
```

The key filename's *KeyType* corresponds to the digital signature algorithm used in the key's creation. The different algorithm types you may see on your system are as follows:

+   - rsa (Rivest–Shamir–Adleman) is the oldest type, widely used, and highly supported.
+   - dsa (Digital Signature Algorithm) is a Federal Information Processing Standard for digital signatures. Deprecated.
+   - ecdsa (Elliptical Curve Digital Signature Algorithm) is an Elliptic Curve implementation of DSA.
+   - ed25519 is a variation of the Edwards-curve Digital Signature Algorithm (EdDSA) that offers better security than dsa and ecdsa.

> **NOTE**
> 
> It is critical that the private key files are properly protected. Private key files should have a 0640 or 0600 (octal) permission setting and be root owned. However, public key files need to be world readable. File permissions were covered in Chapter 4.

There may be times you need to manually generate these keys or create new ones. To do so, use the `ssh-keygen` utility. In Listing 10.36, a snippeted example of using ssh-keygen is shown.