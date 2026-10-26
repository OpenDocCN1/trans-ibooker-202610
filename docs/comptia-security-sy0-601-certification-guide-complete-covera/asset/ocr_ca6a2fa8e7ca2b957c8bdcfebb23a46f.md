# Load Balancer Scheduling

Scheduling is how the load is distributed by the load balancer, let's look at these options in turn:

+   - **Least Utilized Host:** The benefits of a load balancer are that it knows the status of all web servers in the server farms and knows which web servers are the least utilized by using a scheduling algorithm.

**Example:** The load balancer (see *Figure 7.13*) has selected to send the request to Web 3, which has the least number of requests (50), and Web 4 will not be considered as it is currently offline. A user requesting three different pages may obtain them from different web servers but may not know this as the load balancer is optimizing the delivery of the web pages to the user.

+   - **Affinity:** When the load balancer is set to **Affinity**, the request is sent to the same web server based on the requester's IP address. This is also known as *persistence* or a *sticky session*, where the load balancer uses the same server for the session.

+   - **DNS Round Robin:** While using DNS round-robin, when the request comes in, the load balancer contacts the DNS server and rotates the request based on the lowest IP address first. It rotates around Web 1, Web 2, and Web 3, and then keeps the sequence going by going back to Web 1 on a rotational basis:

![](bbox=[0.298, 0.519, 0.721, 0.871])

Figure 7.14 – DNS Round Robin