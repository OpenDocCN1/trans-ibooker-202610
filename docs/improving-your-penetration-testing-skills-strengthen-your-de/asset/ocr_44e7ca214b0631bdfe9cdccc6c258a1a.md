# hash-identifier

Kali Linux includes a tool called hash-identifier, which has a long list of hash patterns and is very useful to determine the type of hash involved:

```
root@kali:~# hash-identifier
######################################################################
#                                                                    #
#                                                                    #
#                                                                    #
#                                                                    #
#                                                                    #
#                                                                    #
######################################################################
HASH: 6f0b5c34cbd9d66132b7d3a4484f1a9af02965904de38e3e3c4e6667d948f20bd0b5b3ebcac9fdbd2f89b76cfde5b0a0ad9c06bccbc662be420b877c080e8fe
Possible Hashes:
[+] SHA-512
[+] Whirlpool
Least Possible Hashes:
[+] SHA-512(HMAC)
[+] Whirlpool(HMAC)
HASH: e76a46033c5a60454c11bddd6c980668
Possible Hashes:
[+] MD5
[+] Domain Cached Credentials - MD4(MD4({$pass})). (strtolower($username))
Least Possible Hashes:
[+] RAdmin v2.x
[+] NTLM
[+] MD4
[+] MD2
[+] MD5(HMAC)
[+] MD4(HMAC)
```