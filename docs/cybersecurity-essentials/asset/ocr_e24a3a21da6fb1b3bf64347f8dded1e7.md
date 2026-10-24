# Hands-On Exercises

# Objectives

The purpose of this lab is to implement file hashes, compare file hashes, describe the usage and importance of file hashing, and define file integrity.

# Resources

+   1. Customer-supplied desktop/laptop hardware system
+   2. Windows 10 Professional installed
+   3. MD5deep installed

MD5deep may be obtained from https://sourceforge.net/projects/md5deep/files/md5deep/md5deep-4.3/md5deep-4.3.zip/download. Download, extract, and move the MD5deep folder to your desktop.

# Discussion

*Hashing* is a basic type of cryptography. It employs a one-way algorithm that creates a unique hash (also known as a *message digest*). The purpose of a hash is not to create a cryptographic message to be decrypted later; instead, it is used to compare a file before and after a certain set of circumstances has arisen.

Hashing does not ensure confidentiality, rather it confirms file integrity. File integrity proves the file has not been altered in any way while the file or message was in transit.

A common use of file hashing is used with downloads from the Internet. Perhaps a freeware item is available, but you are worried that a malicious user has tampered with the file on the site. The owner of the site, however, has provided a hash of the original file in case you want to compare it once downloaded. Examining the string of characters on the site, you create a hash of the downloaded file and compare. If they are exactly the same, you have confirmed file integrity.

# Procedures

In this procedure, you will create and compare hashes. You will also tamper with the file to witness the differences in hashing so you'll better understand what ensuring file integrity really means.