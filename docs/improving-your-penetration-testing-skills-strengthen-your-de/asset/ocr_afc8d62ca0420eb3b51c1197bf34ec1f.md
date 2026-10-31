This first screenshot shows the result of ssl-enum-ciphers, displaying the ciphers allowed for SSLv3. In the next screenshot, the ssl-heartbleed script shows that the server is vulnerable:

```
ssl-heartbleed:
  VULNERABLE:
    The Heartbleed Bug is a serious vulnerability in the popular OpenSSL cryptographic software library. It allows for stealing information intended to be protected by SSL/TLS encryption.
    State: VULNERABLE
    Risk factor: High
    OpenSSL versions 1.0.1 and 1.0.2-beta releases (including 1.0.1f and 1.0.2-beta1) of OpenSSL are affected by the Heartbleed bug. The bug allows for reading memory of systems protected by the vulnerable OpenSSL versions and could allow for disclosure of otherwise encrypted confidential information as well as the encryption keys themselves.
    References:
    https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-0160
    http://www.openssl.org/news/secadv_20140407.txt
    http://cvedetails.com/cve/2014-0160/
```

Also, the ssl-poodle script identifies the server as vulnerable to POODLE:

```
ssl-poodle:
  VULNERABLE:
    SSL POODLE information leak
    State: VULNERABLE
    IDs: CVE-CVE-2014-3566 OSVDB:113251
    The SSL protocol 3.0, as used in OpenSSL through 1.0.1i and other products, uses nondeterministic CBC padding, which makes it easier for man-in-the-middle attackers to obtain cleartext data via a padding-oracle attack, aka the "POODLE" issue.
    Disclosure date: 2014-10-14
    Check results:
      TLS_RSA_WITH_AES_128_CBC_SHA
    References:
    https://www.openssl.org/~bodo/ssl-poodle.pdf
    https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2014-3566
    https://www.imperialviolet.org/2014/10/14/poodle.html
    http://osvdb.org/113251
MAC Address: 08:00:27:06:68:C5 (Oracle VirtualBox virtual NIC)
```

# Exploiting Heartbleed

Heartbleed was discovered in April 2014. It consists of a buffer over-read situation in the OpenSSL TLS implementation; that is, more data can be read from memory than should be allowed. This situation allows an attacker to read information from the OpenSSL server's memory in cleartext. This means that there is no need to decrypt or even intercept any communication between client and server; you simply ask the server what's in its memory and it will answer with the unencrypted information.