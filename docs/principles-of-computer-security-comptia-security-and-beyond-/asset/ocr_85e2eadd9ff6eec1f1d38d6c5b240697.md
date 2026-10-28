the hash of the name that doesn’t exist and what hash comes after the hash of the name that doesn’t exist.

![](bbox=[0.185, 0.15, 0.815, 0.65])

# FIGURE 7-18 Hashing the before and after resource records

All the attacker needs to do now, to get the actual names, is an offline dictionary attack or brute force attack (both of which you’ll perform in Chapter 11). Subdomains/hostnames are not that long. If an attacker precomputes a wordlist for all subdomains/hostnames of length 10 and below, a corresponding hash for each name will be stored. Now the attacker simply compares the two hashes in the NSEC3 resource record with the stored hashes to find out what the subdomain/hostname is for each. Interestingly enough, three NSEC3 resource records will actually be returned, for reasons explained in sections 5.5 and 5.6 of RFC 7129