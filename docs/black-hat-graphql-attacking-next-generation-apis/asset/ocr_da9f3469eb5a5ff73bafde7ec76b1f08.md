Applications are often developed under a relaxed trust model. Such a trust model assumes that input coming into the GraphQL API from other internal systems on the same network is safe, while input that originates from external sources is unsafe. This approach is very common, but designing systems in this way can backfire; if we are able to hack a system and send commands to another host on the network, we could easily move laterally to other servers. Figure 8-1 illustrates a similar scenario.

![](bbox=[0.22, 0.22, 0.82, 0.42])

Figure 8-1: Network trust boundaries

This figure describes a penetration test in which we’ve identified an internet-facing GraphQL API server, the public server. This server happens to be dual-homed, meaning it has two network interfaces and is part of two separate networks. The server is vulnerable to injection attacks, as it does not adequately check the incoming queries it receives from clients.

Now, imagine that the internal server in the diagram is also a GraphQL server designed to trust any incoming queries from systems on the same network. It was configured in this way because it is not internet facing, and the security architects, in their threat model, assumed the local network was safe. Yet if the public server is hacked, an attacker could send malicious queries to the internal server.

This is why it is important to always perform security checks on any user input. It is also why it’s crucial for hackers to test for injection vulnerabilities wherever we identify that input is allowed.

# The OWASP Top 10

Every few years, OWASP releases new vulnerability class rankings for web applications under the OWASP Top 10 project to help companies focus their security mitigation efforts on the most prevalent software flaw classes.

The injection vulnerability class has been on the OWASP Top 10 list for almost two decades. In the latest OWASP Top 10 release, injection vulnerability was ranked third, as shown in Table 8-1.