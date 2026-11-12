Here is an example of this command:

```
Router> traceroute 65.32.13.33
Type escape sequence to abort.
Tracing the route to 65.32.13.33
  1 10.98.240.1 20 msec 24 msec 16 msec
  2 65.32.15.254 16 msec 16 msec 12 msec
  3 65.32.13.33 12 msec 12 msec 12 msec
```

In this example, the destination was three hops away—each hop is listed on a separate line. For each destination, three tests are performed, where the round-trip time is displayed for each test. If you don’t see a round-trip time, typically indicated by an asterisk, this indicates a possible problem or timeout in the response (for example, if ICMP is blocked at the firewall). Table 8-6 shows other values that you may see instead of the round-trip time.

| Traceroute Output | Explanation |
|---|---|
| * | Either the wait timer expired while waiting for a response or the device did not respond at all potentially because of a firewall blocking ICMP traffic. |
| A | The trace packet was filtered by a remote device (administratively prohibited). |
| U | The port of the device is unreachable (the destination received the trace packet but discarded it). |
| H | The destination is unreachable (the destination segment was reachable, but not the host). |
| I | The user interrupted the traceroute process. |
| N | The network is unreachable (the destination segment was not reachable). |
| P | The protocol is unreachable (the device doesn't support traceroute). |
| Q | The source quench tells the source to slow its output. |
| T | The trace packet exceeded the configured timeout value. |
| ? | The device couldn't identify the specific trace types in the trace packet. |

Table 8-6 Traceroute Messages