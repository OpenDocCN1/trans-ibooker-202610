Application container threads are managed in a thread pool, essentially a Java Executor, which by default in Tomcat is a minimum size of 25 threads and a maximum of 200. If there are no available threads to handle a request, the container maintains them in a queue of runnable tasks and dispatches these as soon as a thread becomes available. This queue by default is size Integer.MAX_VALUE – that is, essentially unbounded¹². By default, if a thread remains idle for 60 seconds, it is killed to free up resources in the Java Virtual Machine.

![](bbox=[0.15, 0.3, 0.85, 0.6])

Figure 5-3. Anatomy of a Web application server

For each request, the method that corresponds with the HTTP request is invoked in a thread. The servlet method processes the HTTP request headers, executes the business logic, and constructs a response that is marshalled by the container back into a TCP/IP packet and sent over the network to the client.

In processing the business logic, servlets often need to query an external database. This requires each thread executing the servlet methods to obtain a database connection and execute database queries. In many databases, especially relational ones, connections are limited resources as they consume memory and system resources in both the client and database