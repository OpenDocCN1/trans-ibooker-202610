to simple guessing, especially if the username is easily obtainable as is the case with email addresses and sequential ID usernames.

Challenge-Handshake Authentication Protocol (CHAP) creates a random string, a challenge phrase, or a secret. It then combines that with something else, such as the username or host name, and sends the combined information to the requesting system. The requestor, in turn, hashes the string and returns the result. The server then checks to see that the hashed result is correct and authenticates or denies the requestor.

With CHAP, the server might make this request periodically and use different challenge phrases to revalidate the connection. Because the secret must be known to both sides and is never sent over the connection, this method is much more secure. Every possible secret must be maintained in plaintext at each end, so this protocol does have its limitations.

Kerberos authentication gets its name from a mythological three-headed dog. Kerberos involves relying on a trusted third-party Ticket Granting Server (TGS) to authenticate client/server interaction. The exact process varies with implementation, but essentially the client exchanges cleartext information with the Authentication Server (AS), which then uses keys shared with the client to encrypt messages that include keys shared between the AS and the TGS, as illustrated in Figure 22.1.

![](bbox=[0.22, 0.53, 0.85, 0.78])

# FIGURE 22.1 Kerberos Authentication

The client then validates these messages and decrypts them using the shared key and encrypts messages that include the TGS keys with keys shared between