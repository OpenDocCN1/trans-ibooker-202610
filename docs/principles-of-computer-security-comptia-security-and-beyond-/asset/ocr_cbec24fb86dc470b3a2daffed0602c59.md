```
jonathan@kali-weissman:~$ dig +dnssec ds net.

; <<>> DiG 9.16.3-Debian <<>> +dnssec ds net.
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 54684
;; flags: qr rd ra ad; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags: do; udp: 512
;; QUESTION SECTION:
;net.                     IN      DS

;; ANSWER SECTION:
net.                    37034   IN      DS      35886 8 2 7862B27F5F516EBE1968044 4D4CE5E762981931842C465F00236401D 8BD973EE
net.                    37034   IN      RRSIG   DS 8 1 86400 20200726050000 20200713040000 46594 . x3VQM QziY0gcEx7eWW5XIkS wu8k5Cp+Fxcwdtzvoe9pWxTrty9DcqSJG Twipf2 fEP8iFvsCRwO+UHQ1BSy4qa75hhhWYzl97E5WAi4tP/OSrC7Uv J8PBuEB463e3Qwmac0r28uyKuwqx7e YtgAlb3Mwptu/gym/UNdebhC4/ wgNq6Er7qvJFMFY7Nkn0EzzxudQnze1hbklOQtJz0fbbaExxHXZP1N TA KpaMdj dAuA35ZLSD o++OXnT08bgpAJjUb0jb3XPWLoKoT9cQmvc7U81o qzKB10798RKRZBz36nS+d grukn+1fhux8Y3Ss9YTTBzGBBaevXEuRz0Y j+40vA=

;; Query time: 24 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
;; WHEN: Mon Jul 13 23:40:56 EDT 2020
;; MSG SIZE  rcvd: 367
```

FIGURE 7-7 The DS resource record that validates the DNSKEY resource record of .net

Step 7 Now, the RIT DNS server would request that the root zone send its DNSKEY resource records. To do that manually, enter

```
dig +dnssec dnskey .
```