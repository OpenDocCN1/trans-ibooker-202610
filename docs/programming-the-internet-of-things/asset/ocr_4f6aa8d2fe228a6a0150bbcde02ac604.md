*Figure P-1. - Notional IoT System Architecture*

I find it easiest to qualify these devices as follows:

+   - *Constrained devices* only handle sensing, actuation, or perhaps both, while only processing messages for themselves, passing messages along if the right communications protocol is implemented, and sending messages to a gateway device. In short, they’re limited in their abilities, and don’t connect directly to the Internet – only via a gateway device.
+   - *Gateway devices* are a super set – they can potentially do the same work as a constrained device (in that they may have sensor(s) or actuator(s) attached), but also have the ability to perform ‘at the edge’ analytics, determine how messages should be routed (if at all), and of course, connect directly to the Internet and the various cloud services that make the IoT useful for business stakeholders.

Can a constrained device connect directly to the Internet? Sure, if it contains a TCP/IP stack, has a routable IP address that’s accessible to and from the public Internet, and of course has the appropriate communications hardware to talk to, and is connected with, the Internet. For the purposes of this book, however, I’ll narrow the category of constrained devices to these two limitations:

+   - They do not support packet routing directly to or from the public Internet (although we’ll assume they support both TCP/IP and UDP/IP) and must interact with a gateway device in order to be part of any IoT ecosystem.