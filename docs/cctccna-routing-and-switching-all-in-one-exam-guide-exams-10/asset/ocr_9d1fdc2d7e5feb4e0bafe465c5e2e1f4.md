| Ping Output | Explanation |
|---|---|
| . | A response was not received before the timeout period expired. |
| ! | A response was received within the timeout period. |
| U | A remote router responded that the destination is unreachable—the network segment is reachable, but not the host. |
| N | A remote router responded that the network is unreachable—the network cannot be found in the routing table. |
| P | A remote device responded that the protocol is not supported. |
| Q | The source quench tells the source to slow its output. |
| M | The ping packet needed to be fragmented, but a remote router couldn't perform fragmentation. |
| A | The ping packet was filtered by a device with an access control list (administratively prohibited). |
| ? | The ping packet type is not understood by a remote device. |
| & | The ping exceeded the maximum number of hops supported by the routing protocol. |

Table 8-5 Output Codes for the ping Commands

![](bbox=[0.13, 0.57, 0.25, 0.65])

**EXAM TIP** Be familiar with the output descriptors used by the ping command in Table 8-4 for both the Cisco CCT and CCNA exams.

![](bbox=[0.13, 0.75, 0.26, 0.83])

**NOTE** You might see a period (.) in the output for a couple of reasons: either a response was received after the timeout period, or no response