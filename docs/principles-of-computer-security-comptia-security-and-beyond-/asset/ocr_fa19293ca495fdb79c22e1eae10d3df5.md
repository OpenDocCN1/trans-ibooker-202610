```
C:\Windows\System32\cmd.exe
Microsoft Windows [Version 10.0.19041.508]
(c) 2020 Microsoft Corporation. All rights reserved.

C:\Users\jswics\Downloads\Strings>strings64 Lab01-01.dll | more

Strings v2.53 - Search for ANSI and Unicode strings in binary images.
Copyright (C) 1999-2016 Mark Russinovich
Sysinternals - www.sysinternals.com

!This program cannot be run in DOS mode.
Rich
.text
`.rdata
@.data
.reloc
SUV
h8`
h8`
L$xQh
h(
RVf
D$"
-(
-- More --
```

# FIGURE 15-1 Running strings64

To advance line by line, press ENTER. To advance page by page, press the spacebar.

You’ll notice some strings that seem to be gibberish, like Rvf, D$\D, _^[], and more. These are just the ASCII/Unicode representation of consecutive bytes that have printable characters. However, these strings are completely worthless from the reversing perspective.