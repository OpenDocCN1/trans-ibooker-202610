Amap Log -m
Appscan
Burp Session XML
Burp Issue XML
...
Qualys Asset XML
Qualys Scan XML
Retina XML
Spiceworks CSV Export
Wapiti XML

# How to do it...

+   1. To test the db_import command, we will use the nmap command, a free security scanner, port scanner, and network exploration tool, with the -oX option to save the result to an XML file. Here is the syntax used to scan the Metasploitable 3 target machine:
+   2. To import the scan report, you can use the db_import command followed by the path to the report you want to import:

```
nmap -Pn -A -oX report 192.168.216.10
```

```
msf > db_import /root/report
[*] Importing 'Nmap XML' data
[*] Import: Parsing with 'Nokogiri v1.8.0'</strong>
[*] Importing host 192.168.216.10
[*] Successfully imported /root/report
```

Alternatively, you can run the db_nmap command directly from msfconsole, and the results will be saved in your current database. The db_nmap command works the same way as the regular nmap command:

```
msf > db_nmap -Pn -A 192.168.216.129
[*] Nmap: Starting Nmap 7.60 ( https://nmap.org ) at
2017-10-17 05:05 EDT
[*] Nmap: Nmap scan report for 192.168.216.129
[*] Nmap: Host is up (0.00092s latency).
[*] Nmap: Not shown: 977 closed ports
[*] Nmap: PORT STATE SERVICE VERSION
[*] Nmap: 21/tcp open ftp vsftpd 2.3.4
[*] Nmap: |_ftp-anon: Anonymous FTP login allowed (FTP code 230)
```