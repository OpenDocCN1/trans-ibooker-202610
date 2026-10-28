ws2_32.dll are called by ordinal (number), not by name, which is why we don’t see the names of those functions here.

A simple Google search brings you to the Microsoft Docs page, which contains all Winsock functions:

https://docs.microsoft.com/en-us/windows/win32/winsock/winsock-functions

2a

2b–2d

# Step 2 Analyze Lab01-01.exe with Strings and form a hypothesis about the binary.

+   a. Let’s take a look at Lab01-01.exe with the following command:
+   strings64 -n 4 Lab01-01.exe | more

+   b. Which functions, visible as strings, deal with finding, opening, and manipulating files?

+   c. Which string indicates a type of file that this malware might be searching for?

+   d. Which string appears to be spoofing an actual file, by deviously altering one character? What are two other problems with that string? What is the legitimate string, also seen in the strings output, that the spoofed string is trying to masquerade as?

3a

3b, 3c

# Step 3 Analyze Lab01-02.exe with Strings and form a hypothesis about the binary.

+   a. Run strings64 on Lab01-02.exe with the following command:
+   strings64 -n 4 Lab01-02.exe | more

This binary appears to be packed—in other words, obfuscated to hide strings from malware analysts like us! A packed binary will at least