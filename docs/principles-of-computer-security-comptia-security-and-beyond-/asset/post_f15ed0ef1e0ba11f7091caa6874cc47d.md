
just shut down completely to prevent the analyst from figuring out more about the binary specimen. That’s why some people jokingly say, “Just run everything in a VM. Malware will detect the VM and shut itself down.”

There are many ways malware can detect a VM. MAC addresses will start with a known organizationally unique identifier (OUI) of a VM vendor. Various processes running in the background are indicative of a VM. There will be registry keys related to a VM. Tools for a VM can be detected. Various RAM structures will have indicators of a VM in use. A BIOS serial number can identify a VM. There are various hardware parameters (serial numbers, motherboard information, and CPU values) that are specific to a VM, compared to a physical machine. Malware can even manipulate the assembly language IN instruction, which can put a special value in a CPU register that indicates that a VMware VM is in use.

Advanced static analysis is when analysts get down and dirty with the malware. This type of analysis uses a program known as a disassembler, very often IDA (the Interactive Disassembler), which turns the binary’s ones and zeros into assembly language, allowing analysis of the assembly code itself. Advanced dynamic analysis involves running the malware though a program known as a debugger, such as x64dbg, which lets analysts examine RAM and CPU registers as the program executes. This type of analysis enables analysts to work through certain instructions one step at a time, pausing the program at breakpoints and investigating each instruction. The lab exercises in this chapter will deal with basic static and basic dynamic analysis, not advanced static nor advanced dynamic analysis, which require a strong background in assembly language.

When I teach programming courses on C++, Java, Python, and other languages, I tell my students that when they submit their high-level code, the code must be well documented. They must be able to explain what every single line of code does. When I teach malware analysis, also known as malware reverse engineering, I say the opposite. Most malware binaries are turned into an enormously large number of lines of assembly code. It’s just impossible to understand what every single line of low-level code does. Besides, that would take a great amount of time. Furthermore, lots of times malware authors will purposely write garbage lines of code that do nothing, just to throw analysts off, pushing them in the wrong directions.

## Note

The two languages used most often to write malware are C and C++ because they have no dependency requirements (for example, Java requires the Java Virtual Machine, and Python requires the Python interpreter), they produce small executables, and they can be used for writing rootkits and performing memory hacking since they are not behind any abstraction layers/levels (unlike other languages).

So what should you do? Rule number one: focus on certain specifics of the binary and look for interesting areas to focus on. Rule number two: use different tools and strategies. Don’t spend too much time in one area using a single tool. Do something different to shake things up. Look at the malware in a different way. Rule number three: don’t forget that cybersecurity and malware analysis is a cat-and-mouse game. Anti-analysis, where the malware authors purposely make the analysis harder, is always an issue. Once the good guys figure out what the bad guys are doing, the bad guys use new techniques to make the analysis harder once again.

## 30 MINUTES

## Lab Exercise 15.01: Strings

When analyzing malware, a great first step is to look at the binary’s strings. A string is a consecutive sequence of characters, including letters, numbers, and symbols. Strings can give malware analysts great insight into what a piece of malware does, how it does what it does, and to where its communications go.

What specifically can strings contain? They can contain names of application programming interfaces (APIs) implemented through dynamic link library (DLL) files. Two DLLs that should always be present are kernel32.dll and user32.dll (even on 64-bit systems, as the name stayed the same for DLLs that have “32” in them).

The kernel32.dll file is used for memory management, input/output operations, and hardware interrupts. Functions of interest found in kernel32.dll include OpenProcess, CreateFileW, WriteFile, and FindNextFileW.

## Note

Windows functions that manipulate characters can come in one of three formats: a generic version that can be used by both Windows code pages (character encodings used in Windows operating systems in the 1980s and 1990s, superseded when Windows implemented Unicode, but still applicable when shortcuts with the ALT key are used) and Unicode, a version just for Windows code pages that ends with the letter A (for ANSI), and a version just for Unicode that ends with the letter W (for wide). Functions that end with Ex (for extended) are updates of existing functions with added parameters that provide additional options and behavior. As the new and old functions aren’t compatible, Microsoft still supports the old function (without the Ex suffix).

The user32.dll file is used to create and control elements of the Windows user interface, such as the desktop, windows, buttons, scrollbars, menus, and user action responses, allowing programs to implement a graphical user interface (GUI) that can match the Windows look and feel. Seeing it in the output of Strings doesn’t say much. Functions of interest found in user32.dll include SetWindowTextW and ShowWindow.

The advapi32.dll file is an advanced Windows 32 base API DLL file (an API services library) that supports security, the service manager, and the registry. Seeing that API could be indicative of something the malware might be doing. A function of interest in advapi32.dll is RegSetValueExW. Certain registry keys in strings can be red flags, like Software\Microsoft\Windows\CurrentVersion\Run, which is a registry key (found in both HKEY_LOCAL_MACHINE and HKEY_CURRENT_USER) commonly used by malware that controls which programs are automatically run when Windows starts up.

The shell32.dll file contains Windows shell functions that could be indicative of what malware could be doing. The gdi32.dll has functions that control graphics device interface drawing and output sent to video displays.

- Step 1 Other than the fact that strings64 is a 64-bit binary file and strings is a 32-bit binary file, there is no difference between the two, and both will produce the same results. The commands will reference strings64, but they could all be run using strings.

## Note

While the vast majority of machines running Windows are 64-bit systems today, most malware is still actually compiled into 32-bit binaries. While 32-bit binaries are backward compatible on 64-bit systems, 64-bit binaries won’t run on 32-bit systems. Since malware adversaries want to infect as many systems as possible, malware is still compiled as 32-bit binaries in large numbers. In fact, in 2017, this whitepaper claimed that over 99% of all Windows malware was 32-bit: https://info.deepinstinct.com/whitepaper-beware-of-the-64-bit-malware The whitepaper explains, though, that 64-bit malware is on the rise, as there are advantages for malware adversaries to compile malware into 64-bit binaries, even if it takes away the backward compatibility.

This article explains the rise in 2020 of Mac malware, although the attacks are limited in scope: https://www.zdnet.com/article/mac-malware-is-growing-fast-but-its-still-not-as-dangerous-as-the-attacks-on-windows/

This article explains that Linux systems are targets for malware as well: https://www.zdnet.com/article/this-surprise-linux-malware-warning-shows-that-hackers-are-changing-their-targets/

Explore basic usage of Strings.

- a. Type strings64 Lab01-01.dll | more, as shown in Figure 15-1.
