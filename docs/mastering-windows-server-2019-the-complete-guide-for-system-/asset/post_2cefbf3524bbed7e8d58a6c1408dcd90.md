
- ipconfig /flushdns 148
### Split-brain DNS 148
### Types of DNS zones 149
  - Active Directory Integrated Zones 150
  - Forward Lookup Zones 150
  - Reverse Lookup Zones 150
  - Primary Zone 152
  - Secondary Zone 153
  - Stub Zone 153
  - Creating a new zone 154
### IP addressing with DHCP 158
### Creating a DHCP scope 159
  - Scope Options 161
### DHCP reservations 163
### DHCP failover 167
  - Two DHCP servers 167
  - Hot standby mode 167
  - Load sharing mode 168
  - Configuring DHCP failover 168
### IPAM 173
### Summary 180
### Questions 180
## Chapter 5: Group Policy 181
### Group Policy Object 182
  - Group Policy background refresh cycle 183
### Building a GPO 184
  - Adding Trusted Sites 185
  - Mapping network drives 187
  - Installing registry keys 191
### Scoping a GPO 193
  - Links 193
    - GPRESULT 194
    - Continuing with the link 195
  - Group Policy processing order 196
    - Local Policy 197
    - Site-level policies 197
    - Domain-level policies 197
    - OU-level policies 198
  - Security Filtering 199
  - WMI Filtering 201
  - Item-level targeting 202
  - Delegation 204
- Computer settings and user settings 207
  - Computer Configuration 207
  - User Configuration 208
  - Linking GPOs accordingly 209
  - Group Policy loopback processing 209
- Policy vs preference 210
  - Policies 210
  - Preferences 211
- Default Domain Policy 211
- Administrative Templates 213
  - Implementing ADMX/ADML files 214
- Central Store 216
  - Enable the Central Store 217
  - Populate the Central Store 217
- Summary 220
- Questions 220
## Chapter 6: Certificates in Windows Server 2019 221
- Common certificate types 222
  - User certificates 223
  - Computer certificates 223
  - SSL certificates 224
    - Single-name certificates 226
    - Multi-domain or subject alternative name certificates 227
    - Wildcard certificates 227
- Planning your PKI 228
  - Role services 228
  - Enterprise versus Standalone 230
  - Root versus subordinate (issuing) 232
  - Naming your CA server 232
  - Can I install the CA role onto a domain controller? 233
- Creating a new certificate template 233
- Issuing your new certificates 237
  - Publishing the template 238
  - Requesting a cert from MMC 241
  - Requesting a certificate from the web interface 244
  - Creating an auto-enrollment policy 248
- Obtaining a public-authority SSL certificate 252
  - Public/private key pair 253
  - Creating a certificate signing request 254
  - Submitting the certificate request 257
  - Downloading and installing your certificate 259
### Exporting and importing certificates 261
- Exporting from MMC 262
- Exporting from IIS 263
- Importing into a second server 264
### Summary 264
### Questions 265
## Chapter 7: Networking with Windows Server 2019 267
### Introduction to IPv6 268
  - Understanding IPv6 IP addresses 270
### Your networking toolbox 274
  - ping 274
  - tracert 276
  - pathping 277
  - Test-Connection 279
  - Telnet 281
  - Test-NetConnection 284
  - Packet tracing with Wireshark 285
  - TCPView 286
### Building a routing table 287
  - Multi-homed servers 288
  - Only one default gateway 288
  - Building a route 290
  - Adding a route with the Command Prompt 290
  - Deleting a route 292
  - Adding a route with PowerShell 293
### NIC Teaming 294
### Software-defined networking 297
  - Hyper-V Network Virtualization 298
  - Private clouds 298
  - Hybrid clouds 300
  - How does it work? 300
    - System Center Virtual Machine Manager 301
    - Network Controller 301
    - Generic Routing Encapsulation 302
    - Microsoft Azure Virtual Network 303
    - RAS Gateway/SDN Gateway 303
  - Virtual network encryption 304
  - Bridging the gap to Azure 305
  - Azure Network Adapter 305
### Summary 308
### Questions 308
## Chapter 8: Remote Access 309
- Always On VPN 310
  - Types of AOVPN tunnels 311
    - User tunnels 312
    - Device tunnels 312
  - Device tunnel requirements 312
  - AOVPN client requirements 313
    - Domain-joined 314
  - Rolling out the settings 314
  - AOVPN server components 316
    - Remote Access server 316
    - Certification Authority (CA) 317
    - Network Policy Server (NPS) 318
- DirectAccess 318
  - The truth about DirectAccess and IPv6 320
  - Prerequisites for DirectAccess 321
    - Domain-joined 321
    - Supported client operating systems 322
    - DirectAccess servers – one or two NICs? 322
    - To NAT or not to NAT? 324
    - Network Location Server 327
    - Certificates used with DirectAccess 329
  - Do not use the Getting Started Wizard (GSW)! 331
- Remote Access Management Console 332
  - Configuration 333
  - Dashboard 335
  - Operations Status 336
  - Remote Client Status 337
  - Reporting 337
  - Tasks 339
- DA, VPN, or AOVPN? Which is best? 339
  - Domain-joined or not? 339
  - Auto or manual launch 340
  - Software versus built-in 340
  - Password and login issues with traditional VPNs 341
  - Port-restricted firewalls 342
  - Manual disconnect 344
  - Native load-balancing capabilities 344
  - Distribution of client configurations 345
- Web Application Proxy 346
  - WAP as AD FS Proxy 347
- Requirements for WAP 348
- Latest improvements to WAP 348
  - Preauthentication for HTTP Basic 348
  - HTTP to HTTPS redirection 349
  - Client IP addresses forwarded to applications 349
  - Publishing Remote Desktop Gateway 350
  - Improved administrative console 350
- Summary 352
- Questions 352
## Chapter 9: Hardening and Security 353
- Windows Defender Antivirus 354
  - Installing Windows Defender Antivirus 355
  - Exploring the user interface 355
  - Disabling Windows Defender Antivirus 358
  - What is ATP, anyway? 358
  - Windows Defender ATP Exploit Guard 359
- Windows Defender Firewall – no laughing matter 362
  - Three Windows Firewall administrative consoles 362
    - Windows Defender Firewall (Control Panel) 362
    - Firewall & network protection (Windows Security Settings) 363
    - Windows Defender Firewall with Advanced Security (WFAS) 366
  - Three different firewall profiles 367
  - Building a new inbound firewall rule 369
  - Creating a rule to allow pings (ICMP) 371
  - Managing WFAS with Group Policy 375
- Encryption technologies 379
  - BitLocker and the virtual TPM 380
  - Shielded VMs 381
  - Encrypted virtual networks 381
  - Encrypting File System 382
  - IPsec 382
    - Configuring IPsec 383
- Azure AD Password Protection 388
- Fine-grained password policy 388
- Advanced Threat Analytics – end of support 393
  - What is (was) ATA? 393
  - Azure ATP 396
- General security best practices 396
  - Getting rid of perpetual administrators 397
  - Using distinct accounts for administrative access 398
  - Using a different computer to accomplish administrative tasks 398
  - Never browse the internet from servers 399
- Role-Based Access Control (RBAC) 399
- Just Enough Administration (JEA) 400
- Disable external RDP...NOW 401
### Summary 402
### Questions 403
## Chapter 10: Server Core 405
### Why use Server Core? 406
- No more switching back and forth 408
### Interfacing with Server Core 409
- PowerShell 411
- Using cmdlets to manage IP addresses 411
- Setting the server hostname 415
- Joining your domain 416
- Remote PowerShell 417
- Server Manager 419
- Remote Server Administration Tools 420
- Accidentally closing Command Prompt 422
### Windows Admin Center for managing Server Core 424
### The Sconfig utility 428
### Roles available in Server Core 432
### Building a Server Core domain controller 433
- Install the AD DS role 433
- Promote this server to a domain controller 434
- Verify that it worked 435
### What happened to Nano Server? 438
### Summary 438
### Questions 439
## Chapter 11: PowerShell 441
### Why move to PowerShell? 442
- Cmdlets 442
- PowerShell is the backbone 444
- Scripting 445
- Server Core 445
### Working within PowerShell 446
- Launching PowerShell 446
- Default execution policy 449
- Restricted 450
- AllSigned 450
- RemoteSigned 450
- Unrestricted 450
- Bypass mode 450
- Using the Tab key 452
