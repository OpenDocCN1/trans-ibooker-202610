double-tagging the frames for different VLANs, or they can imitate a trunking port so that they receive all traffic.

+   - **Deauthentication** A deauthentication attack is common in wireless attacks. The hacker sends a command to disconnect or deauthenticate the client from the wireless network. As a result, the client is forced to reauthenticate with the wireless network where the hacker can capture the reauthentication traffic. The authentication packets can be replayed by the attacker in order to generate more traffic in hopes of cracking the wireless encryption.
+   - **Zero-day attacks** In a zero-day attack, a vulnerability unknown by the software developer is exploited before the developer realizes the problem and issues a patch. Companies can have a team in place to test software and identify vulnerabilities not yet known.

# Password Attacks

An attacker may try to crack the passwords stored in a network account database or a password-protected file. There are three major types of password attacks: a dictionary attack, a brute-force attack, and a hybrid attack:

+   - **Dictionary attack** This attack involves the use of a word list file, which is a list of potential passwords. The attacker uses a program to read and try each of the passwords from the file to break into a system. This is a very quick form of password attack, because no calculations are required: the attacker is simply reading a file.
+   - **Brute-force attack** In a brute-force attack, the attacker uses a program that tries every possible combination of characters to guess a password. This attack is slower than a dictionary attack because a file is not read; instead, the program calculates each possible password.
+   - **Hybrid attack** Like a dictionary attack, a hybrid attack involves a word list file, but it also places numbers at the end of the words to catch passwords that are not dictionary words, in case a password contains a number at the end, which is common. For example, a dictionary attack would not find the password *pass1*, but a hybrid attack might.