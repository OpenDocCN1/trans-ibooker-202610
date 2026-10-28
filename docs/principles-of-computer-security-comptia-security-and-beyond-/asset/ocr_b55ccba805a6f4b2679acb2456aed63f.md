```
(jonathan@KaliLinuxWeissman)-[~]
$ mkdir sgid

(jonathan@KaliLinuxWeissman)-[~]
$ chmod 2767 sgid

(jonathan@KaliLinuxWeissman)-[~]
$ ls -l | grep sgid
drwxrwsrwx 2 jonathan jonathan 4096 Jan 18 17:13 sgid

(jonathan@KaliLinuxWeissman)-[~]
$ chmod 2777 sgid

(jonathan@KaliLinuxWeissman)-[~]
$ ls -l | grep sgid
drwxrwsrwx 2 jonathan jonathan 4096 Jan 18 17:13 sgid

(jonathan@KaliLinuxWeissman)-[~]
$ su jsw
Password:
(jsw@KaliLinuxWeissman)-[/home/jonathan]
$ cd sgid
(jsw@KaliLinuxWeissman)-[/home/jonathan/sgid]
$ touch anemptyfile
(jsw@KaliLinuxWeissman)-[/home/jonathan/sgid]
$ ls -l
total 0
-rw-r--r-- 1 jsw jonathan 0 Jan 18 17:14 anemptyfile
(jsw@KaliLinuxWeissman)-[/home/jonathan/sgid]
$ exit
exit

(jonathan@KaliLinuxWeissman)-[~]
$
```

# FIGURE 2-12 SGID

```
mkdir sgid
```