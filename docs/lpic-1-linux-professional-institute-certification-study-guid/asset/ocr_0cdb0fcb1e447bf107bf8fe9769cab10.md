Another great tool to use is the *dig* command. The *dig* command displays all DNS data records associated with a specific host or network. For example, you can look up the information for a specific host name:

```
$ dig www.linux.org
```

```
; <<>> DiG 9.9.4-RedHat-9.9.4-18.el7_1.5 <<>> www.linux.org
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR
```

Or you can look up DNS data records associated with a specific network service, such as a mail server:

```
$ dig linux.org MX
```

```
; <<>> DiG 9.9.5-3ubuntu0.5-Ubuntu <<>> linux.org MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR
```