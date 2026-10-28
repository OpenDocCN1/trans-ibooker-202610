# DriveCrypt by SecurStar

Advertised as “military strength encryption,” DriveCrypt’s 1344-bit key strength sounds quite robust indeed. As for its algorithms, the software offers about a half a dozen including AES and, rather interestingly, the obsolete DES. How exactly the massive key strength is obtained is unclear, but on paper DriveCrypt seems like a solid Windows-only encryption solution. The software even features support for advanced steganography (as discussed earlier in the book) which is the technique of hiding sensitive data in plain daylight in the form of fake music files and the like.

As for backdoors, SecurStar remain adamant that these are not included in their products. Based in Germany, their claim seems to carry some heft as the country is quite comfortable with cryptographic freedoms. This may or may not change in the future, and if it does, SecurStar have vowed to “move the company accordingly.”

A free 30-day demo version of DriveCrypt is available as of this writing. Download DriveCrypt here: www.securstar.com/en/drivecrypt.html.

# eCryptfs

Not a software suite per se, eCryptfs is in fact a complete cryptographic file system for Linux. This solution is actually a so-called stacked file system, which refers to eCryptfs co-operating with a Linux file system. It has been a part of the Ubuntu distribution since version 9.04. It’s also included in Google’s Chrome Operating System, which is a Linux variant.

eCryptfs adds cryptographic metadata into files, so that any encrypted files can be easily transmitted between hosts. These files will be then decrypted automatically should the proper key/password be in the Linux keyring. eCryptfs utilizes the AES algorithm.