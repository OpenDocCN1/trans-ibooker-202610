
## Identifying encrypted and hashed information

The first step when encountering a custom cryptographic implementation or data that cannot be identified as cleartext, is to define the process to which such data was submitted. This task is rather straightforward if the source code is readily accessible. In the more likely case that it isn’t available, the data needs to be analyzed in a number of ways.

## Hashing algorithms

If the result of a process is always the same length irrespective of the amount of data provided, you may be facing a hashing function. To determine which function, you can use the length of the resulting value:

| Function | Length | Example, hash ("Web Penetration Testing with Kali Linux") |
|---|---|---|
| MD5 | 16 bytes | fbdcd5041c96ddbd82224270b57f11fc |
| SHA-1 | 20 bytes | e8dd62289bcf f206905cf269c06692ef7c6938a0 |
| SHA-2 (256) | 32 bytes | dbb5195ef411019954650b6805bf66efc5fa5fef4f80a5f4afda702154ee07d3 |
| SHA-2 (512) | 64 bytes | 6f0b5c34cbd9d66132b7d3a4484f1a9af02965904de38e3e3c4e66676a9 48f20bd0b5b3ebcac9fdbd2f89b76cfde5b0a0ad9c06bccbc662be420b877c080e8fe |

Notice how the preceding examples represent each byte in a hexadecimal codification using two hexadecimal digits to represent the value of each byte (0-255). For clarification, the 16 bytes in the MD5 hash are fb-dc-d5-04-1c-96-dd-bd-82-22-42-70-b5-7f-11-fc. The eleventh byte (42), for example, is the decimal value 66, which is the ASCII letter B.

Also, it is not uncommon to find hashes in base64 encoding. For example, the SHA-512 hash in the preceding table could also be presented as follows:

```
bwtcNMvZ1mEyt90kSE8amvApZZBN444+PE5mZ2UjyC9C1s+vKyf29L4m3bP31sKCtnAa8y8ZivkILh3wIDo/g==
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_317_0.png)

Base64 is an encoding technique used to represent binary data using only the set of printable ASCII characters, where a base64-encoded byte represents 6 bits from the original byte so that 3 bytes (24 bits) can be represented in base64 with 4 ASCII printable bytes.

## hash-identifier

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

## Frequency analysis

A very useful way to tell if a set of data is encrypted, encoded, or obfuscated is to analyze the frequency at which each character repeats inside the data. In a cleartext message, say a letter for example, the ASCII characters in the alphanumeric range (32 to 126) will have a much higher frequency than slashes or nonprintable characters, such as the Escape (27) or Delete (127) keys.

On the other hand, one would expect that an encrypted file would have a very similar frequency for every character from 0 to 255.

This can be tested by preparing a simple set of files to compare with. Let's compare a plaintext file as base with two other versions of that file: one obfuscated and the other encrypted. First create a plaintext file. Use dmesg to send the kernel messages to a file:

```
dmesg > /tmp/clear_text.txt
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_319_0.png)

You can also apply an obfuscation technique called **rotation**, which replaces one letter by another in a circular manner around the alphabet. We will use *ROT13*, rotating 13 places in the alphabet (that is, a will change to n, b will change to o, and so on). This can be done through programming or using sites such as http://www.rot13.com/:

![](img/6b6e557533100c0e47951d0e5e14c4ee_320_0.png)

Next, encrypt the cleartext file using the OpenSSL command-line utility with the AES-256 algorithm and CBC mode:

```
openssl aes-256-cbc -a -salt -in /tmp/clear_text.txt -out
/tmp/encrypted_text.txt
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_321_0.png)

As you can see, OpenSSL's output is base64 encoded. You will need to take that into account when analyzing the results.

Now, how is a frequency analysis performed on those files? We will use Python and the Matplotlib (https://matplotlib.org/) library, preinstalled in Kali Linux, to represent graphically the character frequency for each file. The following script takes two command-line parameters, a file name and an indicator, if the file is base64 encoded (1 or 0), reads that file, and decodes it if necessary. Then, it counts the repetitions of each character in the ASCII space (0-255) and plots the character count:

```
import matplotlib.pyplot as plt
import sys
import base64

if (len(sys.argv))<2:
```

```
print "Usage file_histogram.py <source_file> [1|0]"

print "Reading " + sys.argv[1] + "..."
s_file=open(sys.argv[1])

if sys.argv[2] == "1":
    text=base64.b64decode(s_file.read())
else:
    text=s_file.read()

chars=[0]*256
for line in text:
    for c in line:
        chars[ord(c)] = chars[ord(c)]+1

s_file.close()
p=plt.plot(chars)
plt.show()
```

When comparing the frequency of the plaintext (left) and ROT13 (right) files, you will see that there is no big difference—all characters are concentrated in the printable range:

![](img/6b6e557533100c0e47951d0e5e14c4ee_322_0.png)

On the other hand, when viewing the encrypted file's plot, the distribution is much more chaotic:

![](img/6b6e557533100c0e47951d0e5e14c4ee_323_0.png)

## Entropy analysis

A definitive characteristic of encrypted information that helps to differentiate it from cleartext or encoding is the randomness found in the data at the character level. Entropy is a statistical measure of the randomness of a dataset.

In the case of network communications where file is storage based on the use of bytes formed by eight bits, the maximum level of entropy per character is eight. This means that all of the eight bits in such bytes are used the same number of times in the sample. An entropy lower than six may indicate that the sample is not encrypted, but is obfuscated or encoded, or that the encryption algorithm used may be vulnerable to cryptanalysis.

In Kali Linux, you can use ent to calculate the entropy of a file. It is not preinstalled, but it can be found in the apt repository:

```
apt-get update
apt-get install ent
```

As a PoC, let's execute ent over a cleartext sample, for example, the output of dmesg (the kernel message buffer), which contains a large amount of text including numbers and symbols:

```
dmesg > /tmp/in
ent /tmp/in
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_324_0.png)

Next, let's encrypt the same information and calculate the entropy. In this example, we'll use Blowfish with the CBC mode:

```
openssl bf-cbc -a -salt -in /tmp/in -out /tmp/test2.enc
ent /tmp/test2.enc
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_324_1.png)

Entropy is increased, but it is not as high as that for an encrypted sample. This may be because of the limited sample (that is, only printable ASCII characters). Let's do a final test using Linux's built-in random number generator:

```
head -c 1M /dev/urandom > /tmp/out
ent /tmp/out
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_325_0.png)

Ideally, a strong encryption algorithm should have entropy values very close to eight, which would be indistinguishable from random data.

## Identifying the encryption algorithm

Once we have done frequency and entropy analyses and can tell that the data is encrypted, we need to identify which algorithm was used. A simple way to do this is to compare the length of a number of encrypted messages; consider these examples:

- If the length is not consistently divisible by eight, you might be facing a stream cipher, with RC4 being the most popular
- AES is a block cipher whose output's length is always divisible by 16 (128, 192, 256, and so on)
- DES is also a block cipher; its output's length is always divisible by 8, but not always divisible by 16 (as its keystream is 56 bits)
