# Looking at Password Attacks

A very important type of attack to be familiar with is a password attack. A password attack is an attempt by a hacker to figure out the passwords for user accounts stored on the system. This section discusses the different types of password attacks and how to prevent password attacks from occurring in your environment. Note that these are also known as *cryptographic attacks*, which is the term under which you will find them listed in the Security+ objectives.

![](bbox=[0.073, 0.304, 0.478, 0.413])

# Types of Password Attacks

The three major types of password attacks are dictionary attacks, brute-force attacks, and hybrid attacks. You will learn about each of these password attack types in this section along with other password attack concepts such as password spraying, rainbow tables, and plaintext passwords.

![](bbox=[0.073, 0.553, 0.178, 0.616])

**To see an example of a password-cracking attack, check out the video included in the online resources that accompany this book.**

# Dictionary Attack

A dictionary attack involves the hacker using a program that has a list of popular usernames in one text file and a list of words in a language dictionary that are to be tried as passwords in another file. The dictionary file normally contains all of the words in a language and can be downloaded from the Internet. Figure 4-10 displays a dictionary attack tool called NetBIOS Authentication Tool (NAT) and its sample user list file and password list file.

The benefit of a dictionary attack from a hacker’s point of view is it is a very fast and efficient type of attack because all it does is read the contents of a file—there is no mathematical calculation needed on the part of the password-cracking software. The disadvantage of the dictionary attack is that most passwords today are complex passwords in the sense that they require letters, numbers, and symbols. This makes the dictionary attack ineffective because those passwords are not dictionary words.