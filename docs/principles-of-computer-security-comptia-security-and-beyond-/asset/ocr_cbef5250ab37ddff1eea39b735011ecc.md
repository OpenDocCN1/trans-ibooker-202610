comes next in the entry; for example, show i? displays entries that come after show that start with i. Pressing ? after whitespace shows what entries can come after the previous ones; for example, show ? displays entries that come after show. After the results, in both cases, your original entries are placed at the prompt, so you can resume typing as you were before you entered the ? symbol.

+   i. If necessary, select File | Save and then name your file so that you can start the next lab exercise later or go back to this point of a completed version of Lab Exercise 9.01.

60 MINUTES

# Lab Exercise 9.02: Router Configuration

To connect different networks together, a router maintains a table called a routing table that contains destination networks and directions for the router, such as where to send the traffic to next. If the router’s table in RAM doesn’t have knowledge of a destination network that a packet needs to go to, the routing table might have a default route, which means a specific router interface on a different router to send all traffic with unknown destination networks to. That other router will have a better idea of how to get to the destination. Without knowledge of the destination network or a default route, a router will drop a packet and send an ICMP (Internet Control Message Protocol) error message back to the source.

You would never see a PC connected to a router. Connected to a router you’d find either a switch or another router. That little box we all have at home that everyone calls a “router” actually has switch functionality built inside of it. It’s like there’s a switch and router that are interconnected in that box. If you’ve ever connected a desktop to a port on that router, you’ve actually plugged the cable into a switch port, even though we call that little box a router.

→ Note