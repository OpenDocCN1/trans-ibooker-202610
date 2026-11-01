```
actions: -delete -print0 -printf FORMAT -fprintf FILE
FORMAT -print
    -fprint0 FILE -fprint FILE -ls -fls FILE -prune -quit
    -exec COMMAND ; -exec COMMAND {} + -ok COMMAND ;
    -execdir COMMAND ; -execdir COMMAND {} + -okdir
COMMAND ;
```

As usual, the challenge program will only attempt to implement a subset of these options that I’ll demonstrate forthwith using the files in 07_findr/tests/inputs. The following output from tree shows the directory and file structure. Note that -> indicates that d/b.csv is a link to the file a/b/b.csv. A link is a pointer or a shortcut to another file or directory:

```
$ cd 07_findr/tests/inputs
$ tree
.
├── a
│   ├── a.txt
│   └── b
│       ├── b.csv
│       └── c
│           └── c.mp3
├── d
│   ├── b.csv -> ../a/b/b.csv
│   ├── d.tsv
│   ├── d.txt
│   └── e
│       └── e.mp3
├── f
│   └── f.txt
└── g.csv

6 directories, 9 files
```