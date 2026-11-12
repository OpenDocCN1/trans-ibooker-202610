# TABLE 1.8 The cut command's commonly used options

| Short | Long | Description |
|---|---|---|
| -c nlist | --characters nlist | Display only the record characters in the nlist (e.g., 1-5). |
| -b blist | --bytes blist | Display only the record bytes in the blist (e.g., 1-2). |
| -d d | --delimiter d | Designate the record's field delimiter as d. This overrides the Tab default delimiter. Put d within quotation marks to avoid unexpected results. |
| -f flist | --fields flist | Display only the record's fields denoted by flist (e.g., 1,3). |
| -s | --only-delimited | Display only records that contain the designated delimiter. |
| -z | --zero-terminated | Designate the record end-of-line character as the ASCII character NUL. |

A cut command in action is shown in Listing 1.44.

# **Listing 1.44:** Employing the cut command

```
$ head -2 /etc/passwd
root:x:0:0:root:/root:/bin/bash
bin:x:1:1:bin:/bin:/sbin/nologin
$
$ cut -d ":" -f 1,7 /etc/passwd
root:/bin/bash
bin:/sbin/nologin
[...]
$
```

In Listing 1.44, the head command displays the password file's first two lines. This text file employs colons (:) to delimit the fields within each record. The first use of the cut command designates the colon delimiter using the -d option. Notice the colon is encased in quotation marks to avoid unexpected results. The -f option specifies that only fields 1 (username) and 7 (shell) should be displayed.

Occasionally it is worthwhile to save a cut command's output. You can do this by redirecting standard output, which is covered later in this chapter.