You will often see cipher suites written as **ECDHE-RSA-RC4-MD5**. The format is broken down into the following parts:

+   - **ECDHE**: This is a key exchange algorithm
+   - **RSA**: This is an authentication algorithm
+   - **RC4**: This is an encryption algorithm
+   - **MD5**: This is a hashing algorithm

A comprehensive list of SSL and TLS cipher suites can be found at https://www.openssl.org/docs/apps/ciphers.html.

# SSLScan

Although the OpenSSL command-line tool provides many options to test the SSL configuration, the output of the tool is not user friendly. The tool also requires a fair amount of knowledge about the cipher suites that you want to test.

Kali Linux comes with many tools that automate the task of identifying SSL misconfigurations, outdated protocol versions, and weak cipher suites and hashing algorithms. One of the tools is **SSLScan**, which can be accessed by going to **Applications | Information Gathering | SSL Analysis**.

By default, SSLScan checks if the server is vulnerable to the CRIME and Heartbleed vulnerabilities. The `-tls` option will force SSLScan only to test the cipher suites using the TLS protocol.