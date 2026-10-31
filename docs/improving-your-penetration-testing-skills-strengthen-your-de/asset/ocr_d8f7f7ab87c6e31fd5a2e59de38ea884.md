# Zone transfer using dig

DNS servers usually implement replication (that is, for primary and secondary servers) to improve availability. In order to synchronize the host resolution database from primary to secondary, an operation called zone transfer takes place. The secondary server requests the zone (portion of the domain for which that server is responsible) data from the primary, and this responds with a copy of the database, containing the IP address-hostname pairs that it can resolve.

A misconfiguration in DNS servers allows for anyone to ask for a zone transfer and obtain the full list of resolved hosts of these servers. Using the Domain Internet Groper (dig) command-line tool in Linux, you can try to execute a zone transfer to identify additional hosts in the domain. Zone transfers are done over TCP port 53 and not UDP port 53, which is the standard DNS port.

The dig command-line tool is mainly used for querying DNS servers for hostnames. A simple command such as dig google.com reveals the IP address of the domain and the name of the DNS server that hosts the DNS zone for it (also known as the name server). There are many types of DNS records, such as Mail Exchanger (MX), SRV records, and PTR records. The dig google.com mx command displays information for the MX record.

In addition to the usual DNS tasks, the dig command can also be used to perform a DNS zone transfer.

Let's request a zone transfer to zonetransfer.me, a vulnerable domain made for educational purposes by Robin Wood (DigiNinja). The request is made using the dig command, for the AXFR (zone transfer) register of the zonetransfer.me domain to the nsztm1.digi.ninja server:

```
$ dig axfr zonetransfer.me @nsztm1.digi.ninja
```