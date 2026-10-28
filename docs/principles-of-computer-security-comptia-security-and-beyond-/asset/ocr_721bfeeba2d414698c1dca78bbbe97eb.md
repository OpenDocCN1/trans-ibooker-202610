# FIGURE 7-4 arin.net’s DNSKEY resource records that validate the RRSIG resource record

Each zone has two key pairs: a zone signing key (ZSK) and a key signing key (KSK). The ZSK signs every resource record in a zone (creating RRSIG resource records for each group), with the exception of the DNSKEY resource records, which are signed by the KSK (creating an RRSIG resource record for the DNSKEY group). The keys that sign are private keys. The DNSKEY resource records contain the corresponding public keys.

ZSKs and KSKs are really public/private key pairs. The private key of the ZSK pair signs all non-DNSKEY resource records of the zone. That ZSK private key’s corresponding public key is stored in a DNSKEY resource record. The private key of the KSK pair signs DNSKEY resource records. That KSK private key’s corresponding public key is stored in a different DNSKEY resource record.

It’s difficult to swap out an old or compromised KSK. Changing the ZSK is a lot easier. A smaller ZSK can be used, without compromising the security of the server, minimizing the amount of data that the server has to send with each response. A larger KSK can be used and kept separately, so the parent zones don’t have to frequently update the DS resource records (discussed shortly). Furthermore, the DNSKEY resource record set and corresponding RRSIG resource records can be cached for future use.

DNSKEY resource records that have the value 256 contain ZSKs (public keys), while DNSKEY resource records that have the value 257 contain KSKs (public keys as well). In Figure 7-4, you can see that arin.net sends two of each type of key.

RFCs and experts recommend replacing KSKs over a period of years and replacing ZSKs over a period of months. During this short rollover period, both the new and old keys should be in the zone (and do “double signing”), as this enables old keys to time out and be removed from caches.

In other cases, the zone could be prepublishing for rollover, future keys (one future ZSK and one future KSK) that are not yet used for signing.

In the current story, if the decrypted hash matches the computed hash by the RIT DNS server, the DNSKEY resource record validates the RRSIG resource record, and the RRSIG resource record validates the A resource