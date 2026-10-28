For example, the following figure marks the place of a load balancer in a typical master-slave architecture:

![](bbox=[0.13, 0.155, 0.87, 0.385])

Figure 19.2 – Load balancer in a master-slave architecture

By dividing the work across the machines, load balancing strives to achieve maximum throughput and response time.

# Coding challenge 7 – Sticky session

Problem: What is sticky session (or session affinity)?

Solution: Sticky session (or session affinity) is a notion encountered in a load balancer. Typically, the user information is stored in the session, and the session is replicated on all the machines from the cluster. But session replication (see Coding challenge 11) can be avoided by serving a particular user session requests from the same machine.

For this, the session is associated with a machine. This happens when the sessions are created. All the incoming requests for this session are always redirected to the associated machine. The user data is only on that machine.

In Java, sticky session is typically done via the j sessionid cookie. At the first request, the cookie is sent to the client. For each subsequent request, the client request contains the cookie as well. This way, the cookie identifies the session.

The main drawback of the sticky session approach consists of the fact that if the machine fails then the user information is lost, and that session is unrecoverable. If the client browser doesn’t support cookies or cookies are disabled, then sticky session via cookies cannot be achieved.