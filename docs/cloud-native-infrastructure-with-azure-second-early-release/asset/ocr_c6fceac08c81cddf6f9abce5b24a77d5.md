2020-12-16 15:38:27 +0900 is the time, which is specified by input plug-ins and has to be in Unix time format; test.root is the tag (according to Fluentd, it is strongly recommended that the tag be in lowercase and a mix of digits and underscores); and {"action":"logout","user":3} is a JSON object.

The input plug-ins are responsible for retrieving the event logs from external sources. Generally, an input plug-in creates a thread, a socket, and a listening socket. Input plug-ins are not just limited to http and forward. Following are some additional plug-ins:

# in_syslog

For retrieving log events via the Syslog protocol on UDP or TCP. Fluentd creates a socket on port 5140; then, all you need to do is set up your syslog daemon to send log events on this socket:

```
<source>
  @type syslog
  port 5140
  bind 0.0.0.0
  tag system
</source>
```

# in_tail

Allows Fluentd to read events from the tail of logfiles, just like the Unix tail -f command:

```
<source>
  @type tail
  path /var/log/httpd-access.log
  pos_file /var/log/td-agent/httpd-access.log.pos
```