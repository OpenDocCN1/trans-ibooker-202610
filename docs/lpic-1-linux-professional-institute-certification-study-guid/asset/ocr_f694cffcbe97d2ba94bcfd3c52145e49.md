If desired, once your software clock is fairly accurate via NTP, you can manually set the hardware clock to its time. Just use the hwclock -systohc command covered earlier in this chapter.

# Using the chrony Daemon

The chrony daemon (chronyd) has many improvements over ntpd. It can keep accurate time even on systems that have busy networks or that are down for periods of time, and even on virtualized systems. In addition, it synchronizes the system clock faster than does ntpd, and it can easily be configured to act as a local time server itself. With few exceptions, most distributions recommend that you employ the chrony service for software clock synchronization.

You'll find on CentOS and other Red Hat–based distros that the chrony program is installed by default, but not enabled on boot (by default). The package name is chrony, and it is available in most distribution repositories. So if you need to install it, you can do so by using the appropriate package management tool (covered in Chapter 2).

You'll find that on Ubuntu, when chrony is installed it is automatically started and enabled on boot. To start it on CentOS, user super user privileges and type **systemctl start chronyd** at the command line. Use the same command again, but replace start with enable to have chrony start at system boot time.

# Configuring the chrony Daemon

The primary configuration file for chrony is the chrony.conf file, and it may be stored in the /etc/ or the /etc/chrony/ directory. Typically, there is no need to modify anything in this configuration file, but you might want to look at a few items.

The configuration file contains, among other directives, the NTP time servers to use. The directive name for setting these is either server or pool. For chrony, the server directive is typically used for a single time server designation, whereas pool indicates a server pool. The /etc/chrony/chrony.conf file pool lines from an Ubuntu distribution are shown in Listing 7.49.

# Listing 7.49: Looking at the pool directives in the /etc/chrony/chrony.conf file

```
$ grep ^pool /etc/chrony/chrony.conf
pool ntp.ubuntu.com iburst maxsources 4
pool 0.ubuntu.pool.ntp.org iburst maxsources 1
pool 1.ubuntu.pool.ntp.org iburst maxsources 1
pool 2.ubuntu.pool.ntp.org iburst maxsources 2
$
```

Notice that the pool settings are similar to the ntpd server settings in the /etc/ntp.conf file. The maxsources parameter is one exception to this similarity. It designates the maximum number of time servers from the designated source. If one of those servers goes down, chrony finds another one to take its place.