If you want to analyze the certificate-related data, use the following command that will display detailed information on the certificate:

```
sslscan --show-certificate --no-ciphersuites www.amazon.com:443
```

The output of the command can be exported in an XML document using the -xml=<filename> option.

Watch out when NULL is pointed out in the names of the supported ciphers. If the NULL cipher is selected, the SSL/TLS handshake will complete and the browser will display the secure padlock, but the HTTP data will be transmitted in cleartext.

# SSLyze

Another interesting tool that comes with Kali Linux, which is helpful in analyzing the SSL configuration, is the SSLyze tool released by iSEC Partners. The tool is hosted on GitHub at https://github.com/iSECPartners/sslyze, and it can be found in Kali Linux at Applications | Information Gathering | SSL Analysis. SSLyze is written in Python.

The tool comes with various plugins, which help in testing the following:

+   - Checking for older versions of SSL
+   - Analyzing the cipher suites and identifying weak ciphers
+   - Scanning multiple servers using an input file
+   - Checking for session resumption support

Using the -regular option includes all of the common options in which you might be interested, such as testing all available protocols (SSL versions 2 and 3 and TLS 1.0, 1.1, and 1.2), testing for insecure cipher suites, and identifying if compression is enabled.