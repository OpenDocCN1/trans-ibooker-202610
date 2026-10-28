Step 1 Other than the fact that strings64 is a 64-bit binary file and strings is a 32-bit binary file, there is no difference between the two, and both will produce the same results. The commands will reference strings64, but they could all be run using strings.

# Note

While the vast majority of machines running Windows are 64-bit systems today, most malware is still actually compiled into 32-bit binaries. While 32-bit binaries are backward compatible on 64-bit systems, 64-bit binaries won’t run on 32-bit systems. Since malware adversaries want to infect as many systems as possible, malware is still compiled as 32-bit binaries in large numbers. In fact, in 2017, this whitepaper claimed that over 99% of all Windows malware was 32-bit: https://info.deepinstinct.com/whitepaper-beware-of-the-64-bit-malware The whitepaper explains, though, that 64-bit malware is on the rise, as there are advantages for malware adversaries to compile malware into 64-bit binaries, even if it takes away the backward compatibility.

This article explains the rise in 2020 of Mac malware, although the attacks are limited in scope: https://www.zdnet.com/article/mac-malware-is-growing-fast-but-its-still-not-as-dangerous-as-the-attacks-on-windows/

This article explains that Linux systems are targets for malware as well: https://www.zdnet.com/article/this-surprise-linux-malware-warning-shows-that-hackers-are-changing-their-targets/

Explore basic usage of Strings.

+   a. Type strings64 Lab01-01.dll | more, as shown in Figure 15-1.