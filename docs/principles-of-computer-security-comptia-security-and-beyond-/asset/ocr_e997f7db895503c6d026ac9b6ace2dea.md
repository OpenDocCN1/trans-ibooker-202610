```
(jonathan@KaliLinuxWeissman)-[~]
$ echo Jonathan Scott Weissman
Jonathan Scott Weissman

(jonathan@KaliLinuxWeissman)-[~]
$ echo Jonathan Scott Weissman > rochester

(jonathan@KaliLinuxWeissman)-[~]
$ cat rochester
Jonathan Scott Weissman

(jonathan@KaliLinuxWeissman)-[~]
$ echo RIT > rochester

(jonathan@KaliLinuxWeissman)-[~]
$ cat rochester
RIT

(jonathan@KaliLinuxWeissman)-[~]
$ echo FLCC >> rochester

(jonathan@KaliLinuxWeissman)-[~]
$ echo SU >> rochester

(jonathan@KaliLinuxWeissman)-[~]
$ cat rochester
RIT
FLCC
SU

(jonathan@KaliLinuxWeissman)-[~]
$ tac rochester
SU
FLCC
RIT

(jonathan@KaliLinuxWeissman)-[~]
$ sort rochester
FLCC
RIT
SU

(jonathan@KaliLinuxWeissman)-[~]
$
```