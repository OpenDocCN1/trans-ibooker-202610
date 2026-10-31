
- **Social engineering:** This type of information gathering is similar to passive information gathering but relies on human error, and the information leaked out in the form of printouts, telephone conversations, incorrect email IDs, and so on. The techniques for utilizing this method are numerous and the ethos of information gathering is very different, hence, social engineering is a category in itself. For example, hackers register domain names that sound similar with spelling mistakes and set up a mail server to receive such erroneous emails. Such domains are known as **Doppelganger Domains;** that is, the evil twin.

The victims of social engineering are tricked into releasing desired information that they do not realize will be used to attack an enterprise network. For example, an employee in an enterprise may be tricked into revealing an employee identification number to someone who is pretending to be someone he/she trusts. While that employee number may not seem valuable to the employee, which makes it easier for him to reveal the information in the first place, the social engineer can use that employee number in conjunction with other information that has been gathered to get closer to finding a way into the enterprise network.

## Passive information gathering with Metasploit

In this chapter, we will analyze the various passive and active techniques of information gathering in detail. From the beginning, we will analyze the most commonly used and most commonly neglected techniques of passive information gathering and in later recipes, we will focus on gaining information through port scanning. Metasploit has several built-in scanning capabilities, as well as some third-party tools integrated with it to further enhance the process of port scanning. We will analyze both the inbuilt scanners, as well as some of the popular third-party scanners which work over the Metasploit Framework. Let's move on to the recipes and start our process of gaining information about our target.

## Getting ready

We will start information gathering with the company domain name, get information about the company, search for subdomains, find targets, check for honeypots, gather email addresses, and much more.

## How to do it...

The Metasploit Framework has several modules for information gathering. In this recipe, you will learn how to use some of these modules. However, I recommend that you explore all the auxiliary modules available in the framework.

## DNS Record Scanner and Enumerator

The DNS Record Scanner and Enumerator auxiliary module can be used to gather information about a domain from a given DNS server by performing various DNS queries, such as zone transfers, reverse lookups, SRV record brute forcing, and other techniques.

- 1. To run the auxiliary module, we use the use command followed by the module we want to use, in this case, auxiliary/gather/enum_dns. Then we can use the info command to display information about the module, such as the authors, basic options, and description, as shown here:

![](img/6b6e557533100c0e47951d0e5e14c4ee_397_0.png)

2. To run the module, we need to set the domain name, and to make it run a bit faster, we will set the thread number to 10:

```
msf > use auxiliary/gather/enum_dns
msf auxiliary(enum_dns) > set DOMAIN packtpub.com
DOMAIN => packtpub.com
msf auxiliary(enum_dns) > set THREADS 10
THREADS => 10
msf auxiliary(enum_dns) > run

...
[+] packtpub.com NS: dns3.easydns.org.
[+] packtpub.com NS: dns2.easydns.net.
[*] Attempting DNS AXFR for packtpub.com from dns1.easydns.com.
W, [2017-10-17T10:04:14.963345 #5091] WARN -- : AXFR query, switching to TCP
...

include:_spf.freshsales.io a:zgateway.zuora.com
include:amazonses.com ~all
[*] querying DNS SRV records for packtpub.com
[*] Auxiliary module execution completed
msf auxiliary(enum_dns) >
```

Looking at the output, we can see that we are able to obtain several DNS records from the target domain.

## There's more...

The DNS Record Scanner and Enumerator auxiliary module can also be used for active information gathering, using its brute forcing capabilities. By setting ENUM_BRT to true, it will brute force subdomains and hostnames via the supplied wordlist, which you can customize by setting the WORDLIST option to the path of your wordlist.

## CorpWatch Company Name Information Search

Gathering company information is essential, and for that, we can use the CorpWatch Company Name Information Search auxiliary module, auxiliary/gather/corpwatch_lookup_name, which will give us the company's name, address, sector, and industry.

To run the auxiliary/gather/corpwatch_lookup_name auxiliary module, we can use Microsoft as the company name and set the limit to 1 to show only the first result:

```
msf > use auxiliary/gather/corpwatch_lookup_name
msf auxiliary(corpwatch_lookup_name) > set COMPANY_NAME Microsoft
COMPANY_NAME => Microsoft
msf auxiliary(corpwatch_lookup_name) > set LIMIT 1
LIMIT => 1
msf auxiliary(corpwatch_lookup_name) > run

[*] Company Information
--------------------------------
[*] CorpWatch (cw) ID: cw_4803
[*] Company Name: MICROSOFT CORP
[*] Address: ONE MICROSOFT WAY, REDMOND WA 98052-6399
[*] Sector: Business services
[*] Industry: Services-prepackaged software
[*] Auxiliary module execution completed
msf auxiliary(corpwatch_lookup_name) >
```

## Search Engine Subdomains Collector

Gathering subdomains is a great way to find new targets, and we can use the Search Engine Subdomains Collector auxiliary module, auxiliary/gather/searchengine_subdomains_collector, to gather subdomains about a domain from Yahoo and Bing.

To gather subdomains from a target domain, we just need to set the target domain. Let's quickly perform a test on packtpub.com and analyze the output:

```
msf > use auxiliary/gather/searchengine_subdomains_collector
msf auxiliary(searchengine_subdomains_collector) > set TARGET packtpub.com
TARGET => packtpub.com
msf auxiliary(searchengine_subdomains_collector) > run

[*] Searching Bing for subdomains from domain:packtpub.com
[*] Searching Yahoo for subdomains from domain:packtpub.com
[+] domain:packtpub.com subdomain: www.packtpub.com
[*] Searching Bing for subdomains from ip:83.166.169.231
[*] Searching Yahoo for subdomains from ip:83.166.169.231
...

[+] domain:packtpub.com subdomain: www1.packtpub.com
[*] Searching Bing for subdomains from ip:83.166.169.231
[*] Searching Yahoo for subdomains from ip:83.166.169.231
```

```
[+] ip:83.166.169.231 subdomain: www.packtpub.com
[+] ip:83.166.169.231 subdomain: www1.packtpub.com
[+] ip:83.166.169.231 subdomain: www2.packtpub.com
[*] Auxiliary module execution completed
```

The Search Engine Subdomains Collector auxiliary module helped us find new targets, such as www.packtpub.com, cdp.packtpub.com, authorportal.packtpub.com, among others.

Now that we have a good idea about the capabilities of some of the basic modules, let's try the big guns.

## Censys Search

Censys is a search engine that enables researchers to ask questions about the hosts and networks that compose the internet. Censys collects data on hosts and websites through daily ZMap and ZGrab scans of the IPv4 address space, in turn maintaining a database of how hosts and websites are configured.

Using the Censys search auxiliary module, we can use the Censys REST API to access the same data accessible through the web interface. The search endpoint allows searches against the current data in the IPv4, top million websites, and certificates indexes, using the same search syntax as the primary site.

To use the Censys Search auxiliary module, you first need to create a free account at the https://censys.io/ website to get your API ID and secret.

To use the Censys Search auxiliary module, we will set the Censys dork to packtpub.com, the search type to ipv4, followed by your secret and API ID, and type run to run the module:

```
msf > use auxiliary/gather/censys_search
msf auxiliary(censys_search) > set CENSYS_DORK packtpub.com
CENSYS_DORK => packtpub.com
msf auxiliary(censys_search) > set CENSYS_SEARCHTYPE ipv4
CENSYS_SEARCHTYPE => ipv4
msf auxiliary(censys_search) > set CENSYS_SECRET
JIxvPzj0RJkqOqd9cFNRYqNkHzH7E3en
CENSYS_SECRET => JIxvPzj0RJkqOqd9cFNRYqNkHzH7E3en
msf auxiliary(censys_search) > set CENSYS_UID ec421f73-d438-1c48-15b3-5de240bef531
```
