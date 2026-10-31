
- Passive reconnaissance versus active reconnaissance 75
- **Information gathering** 75
- Domain registration details 76
- Whois – extracting domain information 76
- Identifying related hosts using DNS 78
- Zone transfer using dig 79
- DNS enumeration 81
- DNSEnum 82
- Fierce 83
- DNSRecon 85
- Brute force DNS records using Nmap 86
- Using search engines and public sites to gather information 86
- Google dorks 87
- Shodan 88
- theHarvester 89
- Maltego 91
- Recon-ng – a framework for information gathering 92
- Domain enumeration using Recon-ng 93
- Sub-level and top-level domain enumeration 94
- Reporting modules 95
- **Scanning – probing the target** 97
- Port scanning using Nmap 98
- Different options for port scan 98
- Evading firewalls and IPS using Nmap 100
- Identifying the operating system 101
- Profiling the server 102
- Identifying virtual hosts 102
- Locating virtual hosts using search engines 103
- Identifying load balancers 104
- Cookie-based load balancer 104
- Other ways of identifying load balancers 105
- Application version fingerprinting 106
- The Nmap version scan 106
- The Amap version scan 107
- Fingerprinting the web application framework 108
- The HTTP header 109
- The WhatWeb scanner 110
- Scanning web servers for vulnerabilities and misconfigurations 111
- Identifying HTTP methods using Nmap 111
- Testing web servers using auxiliary modules in Metasploit 112
- Identifying HTTPS configuration and issues 112
- OpenSSL client 113
- Scanning TLS/SSL configuration with SSLScan 116
- Scanning TLS/SSL configuration with SSLyze 117
- Testing TLS/SSL configuration using Nmap 118
- Spidering web applications 119
- Burp Spider 119
- Application login 123
- Directory brute forcing 123
- DIRB 124
- ZAP's forced browse 125

## Chapter 4: Authentication and Session Management Flaws 126

### Authentication schemes in web applications 127

Platform authentication 127

Basic 127

Digest 129

NTLM 129

Kerberos 129

HTTP Negotiate 130

Drawbacks of platform authentication 130

Form-based authentication 131

Two-factor Authentication 132

OAuth 132

### Session management mechanisms 133

Sessions based on platform authentication 133

Session identifiers 133

### Common authentication flaws in web applications 135

Lack of authentication or incorrect authorization verification 135

Username enumeration 135

Discovering passwords by brute force and dictionary attacks 143

Attacking basic authentication with THC Hydra 144

Attacking form-based authentication 147

Using Burp Suite Intruder 148

Using THC Hydra 153

The password reset functionality 154

Recovery instead of reset 154

Common password reset flaws 155

Vulnerabilities in 2FA implementations 156

### Detecting and exploiting improper session management 157

Using Burp Sequencer to evaluate the quality of session IDs 157

Predicting session IDs 162

Session Fixation 168

### Preventing authentication and session attacks 173

Authentication guidelines 174

Session management guidelines 175

## Chapter 5: Detecting and Exploiting Injection-Based Flaws 177

### Command injection 178

Identifying parameters to inject data 181

Error-based and blind command injection 181

Metacharacters for command separator 182

Exploiting shellshock 184

Getting a reverse shell 184

Exploitation using Metasploit 189

### SQL injection 191

An SQL primer 191

The SELECT statement 192

Vulnerable code 193

- SQL injection testing methodology 194
- Extracting data with SQL injection 197
- Getting basic environment information 199
- Blind SQL injection 202
- Automating exploitation 208
- sqlninja 209
- BBQSQL 211
- sqlmap 212
- Attack potential of the SQL injection flaw 218
- XML injection 218
- XPath injection 218
- XPath injection with XCat 222
- The XML External Entity injection 224
- The Entity Expansion attack 226
- NoSQL injection 228
- Testing for NoSQL injection 229
- Exploiting NoSQL injection 229
- Mitigation and prevention of injection vulnerabilities 231
- Chapter 6: Finding and Exploiting Cross-Site Scripting (XSS) Vulnerabilities 233
- An overview of Cross-Site Scripting 234
- Persistent XSS 236
- Reflected XSS 238
- DOM-based XSS 238
- XSS using the POST method 240
- Exploiting Cross-Site Scripting 241
- Cookie stealing 241
- Website defacing 243
- Key loggers 245
- Taking control of the user's browser with BeEF-XSS 248
- Scanning for XSS flaws 252
- XSSer 252
- XSS-Sniper 254
- Preventing and mitigating Cross-Site Scripting 255
- Chapter 7: Cross-Site Request Forgery, Identification, and Exploitation 257
- Testing for CSRF flaws 258
- Exploiting a CSRF flaw 261
- Exploiting CSRF in a POST request 261
- CSRF on web services 264
- Using Cross-Site Scripting to bypass CSRF protections 267
- Preventing CSRF 272
- Chapter 8: Attacking Flaws in Cryptographic Implementations 273

## A cryptography primer 274

Algorithms and modes 274

Asymmetric encryption versus symmetric encryption 275

Symmetric encryption algorithm 276

Stream and block ciphers 277

Initialization Vectors 277

Block cipher modes 278

Hashing functions 279

Salt values 279

## Secure communication over SSL/TLS 280

Secure communication in web applications 281

TLS encryption process 282

## Identifying weak implementations of SSL/TLS 283

The OpenSSL command-line tool 283

SSLScan 287

SSLyze 289

Testing SSL configuration using Nmap 290

Exploiting Heartbleed 292

POODLE 295

## Custom encryption protocols 296

Identifying encrypted and hashed information 297

Hashing algorithms 297

hash-identifier 298

Frequency analysis 299

Entropy analysis 303

Identifying the encryption algorithm 305

## Common flaws in sensitive data storage and transmission 306

Using offline cracking tools 307

Using John the Ripper 308

Using Hashcat 310

## Preventing flaws in cryptographic implementations 312

## Chapter 9: Using Automated Scanners on Web Applications 313

## Considerations before using an automated scanner 313

## Web application vulnerability scanners in Kali Linux 314

Nikto 315

Skipfish 317

Wapiti 320

OWASP-ZAP scanner 322

## Content Management Systems scanners 325

WPScan 325

JoomScan 327

CMSmap 328

## Fuzzing web applications 329

Using the OWASP-ZAP fuzzer 330

Burp Intruder 336

## Post-scanning actions 342

Chapter 10: Metasploit Quick Tips for Security Professionals 343
  Introduction 344
  Installing Metasploit on Windows 346
    Getting ready 346
    How to do it... 347
  Installing Linux and macOS 347
    How to do it... 348
  Installing Metasploit on macOS 349
    How to do it... 349
  Using Metasploit in Kali Linux 350
    Getting ready 350
    How to do it... 351
    There's more... 353
      Upgrading Kali Linux 353
  Setting up a penetration-testing lab 354
    Getting ready 354
    How to do it... 355
    How it works... 359
  Setting up SSH connectivity 359
    Getting ready 359
    How to do it... 359
  Connecting to Kali using SSH 360
    How to do it... 361
  Configuring PostgreSQL 362
    Getting ready 362
    How to do it... 362
    There's more... 364
  Creating workspaces 365
    How to do it... 365
  Using the database 366
    Getting ready 366
    How to do it... 367
  Using the hosts command 368
    How to do it... 368
  Understanding the services command 370
    How to do it... 371
Chapter 11: Information Gathering and Scanning 374
  Introduction 374
  Passive information gathering with Metasploit 376
    Getting ready 376
    How to do it... 377
      DNS Record Scanner and Enumerator 377
    There's more... 378
      CorpWatch Company Name Information Search 378

- Search Engine Subdomains Collector 379
- Censys Search 380
- Shodan Search 381
- Shodan Honeyscore Client 382
- Search Engine Domain Email Address Collector 382
- Active information gathering with Metasploit 383
- How to do it... 383
- TCP Port Scanner 384
- TCP SYN Port Scanner 385
- Port scanning—the Nmap way 386
- Getting ready 386
- How to do it... 386
- How it works... 389
- There's more... 390
- Operating system and version detection 390
- Increasing anonymity 391
- Port scanning—the db_nmap way 392
- Getting ready 392
- How to do it... 392
- Nmap Scripting Engine 393
- Host discovery with ARP Sweep 394
- Getting ready 394
- How to do it... 395
- UDP Service Sweeper 396
- How to do it... 396
- SMB scanning and enumeration 397
- How to do it... 397
- Detecting SSH versions with the SSH Version Scanner 400
- Getting ready 401
- How to do it... 401
- FTP scanning 402
- Getting ready 402
- How to do it... 403
- SMTP enumeration 403
- Getting ready 404
- How to do it... 404
- SNMP enumeration 404
- Getting ready 405
- How to do it... 405
- HTTP scanning 406
- Getting ready 406
- How to do it... 407
- WinRM scanning and brute forcing 409
- Getting ready 409
- How to do it... 409
