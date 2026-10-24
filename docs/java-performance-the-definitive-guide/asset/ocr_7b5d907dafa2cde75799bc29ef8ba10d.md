In early versions of Java, all I/O was blocking. A thread that attempted to read data from a socket would wait (block) until at least some data was available or the read timed out. More importantly, there is no way to know if data is available on the socket without attempting to read from the socket. So a thread that wanted to process data over a client connection would have to issue a request to read the data, block until data is available, process the request and send back the response, and then return to the blocking read on the socket. This leads to the situation outlined in Figure 10-1.

![](bbox=[0.13, 0.23, 0.84, 0.37])

Figure 10-1. Threads blocking on I/O reads from clients

Blocking I/O requires that the server has a one-to-one correspondence between client connections and server threads; each thread can handle only a single connection. This is particularly an issue for clients that want to use HTTP keepalive to avoid the performance impact of creating a new socket with every request. Say that 100 clients are sending requests with an average 30-second think time between requests, and it takes the server 500 milliseconds to process a request. In that case, an average of fewer than two requests will be in progress at any point, yet the server will need 100 threads to process all the clients. This is highly inefficient.

Hence, when Java introduced NIO APIs that were nonblocking, server frameworks migrated to that model for their client handling. This leads to the situation shown in Figure 10-2.

![](bbox=[0.13, 0.65, 0.84, 0.79])

Figure 10-2. Threads with event notification for reads