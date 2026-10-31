# Getting ready

To use Nessus for the first time, you will have to register and get a registration code from the Nessus website. To test Nessus, you can use **Nessus Home**, which allows you to scan your personal home network (up to 16 IP addresses per scanner). You can download it at https://www.tenable.com/products/nessus-home.

To install Nessus on Kali Linux, on the download page choose the Debian software package file (.deb) for your version 32 or 64 bits, and use the `dpkg -i` command, followed by the Nessus software package file:

```
root@kali:~# dpkg -i Nessus*.deb
...
Unpacking Nessus Core Components...
essusd (Nessus) 6.11.1 [build M20101] for Linux
Copyright (C) 1998 - 2017 Tenable Network Security, Inc

Processing the Nessus plugins...
[########################################]

All plugins loaded (1sec)

- You can start Nessus by typing /etc/init.d/nessusd start
- Then go to https://kali:8834/ to configure your scanner

Processing triggers for systemd (235-2) ...
root@kali:~#
```

Then, start the Nessus services, using the following command:

```
root@kali:~# systemctl start nessusd.service
```

Then open your browser and go to https://kali:8834/ to configure Nessus. To start working with Nessus in `msfconsole`, we will have to load Nessus and then connect it with the server to start our penetration testing.