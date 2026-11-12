
## Using GRUB 2 as the Boot Loader

Since the GRUB2 system was intended as an improvement over GRUB Legacy, many of the features are the same, with just a few twists. For example, the GRUB2 system changes the configuration file name to grub.cfg. Where the file is stored depends on your system's firmware:

- BIOS: The grub.cfg file is stored in the /boot/grub/ or /boot/grub2/ directory. (This allows you to have both GRUB Legacy and GRUB2 installed at the same time.)

- UEFI: The grub.cfg file is stored in the /boot/efi/EFI/distro-name/ directory.

An example of a grub.cfg file location on a UEFI system, which is a CentOS distribution, is shown in Listing 5.4.

### Listing 5.4: GRUB2 configuration file location on a UEFI CentOS distro

```
# ls /boot/efi/EFI/centos/
BOOT.CSV fonts grubenv mmx64.efi shimx64-centos.efi
BOOTX64.CSV grub.cfg grubx64.efi shim.efi shimx64.efi
#
```

## Configuring GRUB2

There are also a few changes to the commands used in GRUB2. A simplified snippet example of a GRUB2 configuration file is shown in Listing 5.5.

### Listing 5.5: Simplified sample GRUB2 configuration file

```
[...]
menuentry "CentOS Linux" {
[...]
set root=(hd1,1)
linux16 /vmlinuz[...]
initrd /initramfs[...]
}
menuentry "Windows" {
set root=(hd0,1)
[...]
```

Notice that GRUB2 uses the set command to assign values to the root keyword, and it uses an equal sign to assign the device. GRUB2 utilizes environment variables to configure settings instead of commands.

To make things more confusing, GRUB2 changes the numbering system for partitions. Although it still uses 0 for the first hard drive, the first partition is set to 1. So to define the /boot directory on the first partition of the first hard drive, you now need to use
set root=(hd0,1)

Sometimes the format is slightly different, where msdos indicates a DOS partition's number or gpt indicates a GPT (covered in Chapter 3) partition's number:

```
set root='hd0,msdos1'
```

For GRUB2, the commands to remember are as follows:

- Menuentry: The first line for each boot definition section; this is what appears in the boot menu.
- set root: Defines the disk and partition where the GRUB2 /boot directory partition is located on the system.
- Linux, linux16: For BIOS systems, defines the kernel image file stored in the /boot directory to load.
- Linuxefi: For UEFI systems, defines the kernel image file stored in the /boot directory to load.
- Initrd: For BIOS systems, defines the initial RAM filesystem, which contains drivers necessary for the kernel to interact with the system hardware.
- Initrdefi: For UEFI systems, defines the initial RAM filesystem, which contains drivers necessary for the kernel to interact with the system hardware.

Notice in Listing 5.5 that you must enclose each individual boot section with braces immediately following the menuentry command. In addition, the rootnoverify command is not used in GRUB2. Non-Linux boot options are now defined the same as Linux boot options using the root environment variable.

> If you need some detailed GRUB guidance, the GNU organization has a nice manual at www.gnu.org/software/grub/manual/grub/grub.html. Be sure to peruse your distribution's GRUB documentation as well.

The configuration process for GRUB2 is also somewhat different. Although GRUB2 uses the /boot/grub/grub.cfg file as the configuration file, you should never modify that file. Instead, there are separate configuration files stored in the /etc/grub.d folder. This allows you (or the system) to create individual configuration files for each boot option installed on your system (for example, one configuration file for booting Linux and another for booting Windows).

For global commands, the /etc/default/grub configuration file is used. Typically, you should not modify this file either, but again use the configuration files in the /etc/grub.d/ directory. The format for some of the global commands has changed from the GRUB Legacy commands, such as GRUB_TIMEOUT instead of just timeout.

Most Linux distributions generate a new grub.cfg configuration file automatically after certain events, such as when upgrading the kernel. Usually, the distribution will keep a boot menu option pointing to the old kernel file, which is handy just in case the new one fails.

## Installing GRUB2

Unlike with GRUB Legacy, you don't need to install GRUB2. All you need to do is to rebuild the main installation file. This is done by running either the grub-mkconfig or grub2-mkconfig program. They are essentially equivalent programs but may not both be installed on your system.

The grub2-mkconfig program reads configuration files stored in the /etc/grub.d folder and assembles the commands into the single grub.cfg configuration file.

You can update the configuration file manually by using super user privileges and running the grub2-mkconfig command:

```
# grub2-mkconfig > /boot/grub2/grub.cfg
```

Notice that you must either redirect the output of the grub2-mkconfig program to the grub.cfg configuration file or use the -o (or --output=) option to specify the output file. By default, the grub2-mkconfig program just outputs the new configuration file commands to standard output.

To further add to your GRUB installation choices, Ubuntu has the update-grub utility. This program issues the command grub-mkconfig -o /boot/grub/grub.cfg for you. It's always nice to save a few keystrokes.

## Interacting with GRUB2

The GRUB2 boot loader produces a boot menu similar to the GRUB Legacy method. Figure 5.1 shows an Ubuntu GRUB2 menu interface. You can use arrow keys to switch between the various boot menu entries.

### FIGURE 5.1 An Ubuntu GRUB2 menu

![](img/ca7617340ddbe2360d36ce3428729fa9_308_0.png)

Use the ↑ and ↓ keys to select which entry is highlighted. Press enter to boot the selected OS, 'e' to edit the commands before booting or 'c' for a command-line.

Some graphical desktops (such as Ubuntu) hide the GRUB boot menu behind a graphical interface. Usually, if you hold down the Shift key when the system first boots, this will display the GRUB boot menu.

If you want to edit a particular boot menu entry, when your cursor is on the appropriate boot option line, press the E key to edit the entry. Figure 5.2 illustrates the editing of an entry in the GRUB2 boot menu on an Ubuntu system.

### FIGURE 5.2 Editing an Ubuntu GRUB2 menu entry

![](img/ca7617340ddbe2360d36ce3428729fa9_309_0.png)

For understanding (and certification exam studying purposes) we've provided you two tables. One is for operating in the GRUB2 menu interface (Table 5.2), and the other displays the various basic GRUB2 entry editor keystrokes (Table 5.3).

### TABLE 5.2 GRUB2 menu interface keystrokes

| Key(s) | Description |
| :--- | :--- |
| Arrow | Select boot menu option |
| C | Starts the GRUB command-line interface |
| E | Enters editing mode for currently selected menu option |
| Enter | Boots currently selected menu option |
| P | Used to enter password, if required |

The GRUB2 entry editor supports basic emacs editing keystrokes (covered in Chapter 1),
which is handy. However, you should be familiar with a few additional items, listed in
Table 5.3.

| Key(s) | Description |
| :--- | :--- |
| Arrow | Highlight the entry line to modify |
| Enter | Adds a new line (best to use at a line's end) |
| ESC | Discards any changes and return to menu interface |
| Ctrl+C | Starts the GRUB command-line interface |
| Ctrl+X | Boot system with edited entry |

## Adding Kernel Boot Parameters

Besides editing a GRUB configuration on the fly from its boot menu, you can provide options to the Linux kernel. This approach is especially useful in troubleshooting booting problems.

To accomplish this using the techniques outlined in the previous sections, edit the appropriate boot menu entry, find the line starting with kernel (GRUB Legacy) or linux* (GRUB2), go to the end of the line, add a space, and then tack on the kernel option(s). After you've completed that, boot the system using the appropriate keystroke.

A few of the more useful kernel parameters are listed in Table 5.4.

| Parameter | Description |
| :--- | :--- |
| console= | Set the console device |
| debug | Enable kernel debugging |
| init= | Execute the specified program, such as a Bash shell (/bin/bash) instead of /sbin/init |
| initrd= | Change the location of the initial RAM filesystem |
| mem | Set the total amount of available system memory |
| ro | Mount root filesystem as read-only |
| root= | Change the root filesystem |
