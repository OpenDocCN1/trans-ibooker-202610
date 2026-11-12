Some distros include the distribution version in the build number. For example, you find el7 (Red Hat Enterprise Linux v7) or fc29 (Fedora, formerly called Fedora Core, v29) after a dot.

**ARCHITECTURE** This is a designation of the CPU architecture for which the software package was optimized. Typically you'll see x86_64 listed for 64-bit processors. Sometimes noarch is used, which indicates the package is architecturally neutral. Older CPU architecture designations include i386 (x86), ppc (PowerPC), and i586 and i686 (Pentium).

> There are two types of RPM packages: source and binary. Most of the time, you'll want the binary package, because it contains the program bundle needed to successfully run the software. A source RPM contains the program's source code, which can be useful for analysis (or for incorporating your own package customizations). You can tell the difference between these two package file types because a source RPM has src as its ARCHITECTURE in the RPM filename.

It's helpful to look at some example RPM files. Listing 2.1 shows four different RPM files we downloaded on a CentOS distribution.

# **Listing 2.1:** Viewing RPM package files on a CentOS distribution

```
```
# ls -l *.rpm
docker-1.13.1-94.gitb2f74b2.el7.centos.x86_64.rpm
emacs-24.3-22.el7.x86_64.rpm
openssh-7.4p1-16.el7.x86_64.rpm
zsh-5.0.2-31.el7.x86_64.rpm
#
```
```

Notice the format naming variations between the version and release numbers. Although it can be difficult to determine where a version number ends and a release number begins, the trick is to look for the second dash (-) in the filename, which separates them.

> If you want to obtain copies of RPM files on a Red Hat–based distro such as CentOS or Fedora, employ the yumdownloader utility. For example, use super user privileges and type **yumdownloader emacs** at the command line to download the emacs RPM file to your current working directory. On openSUSE, you'll need to employ the zypper install -d package-name command, using super user privileges. This will download the RPM package file(s) to a /var/cache/zypp/packages/ subdirectory.