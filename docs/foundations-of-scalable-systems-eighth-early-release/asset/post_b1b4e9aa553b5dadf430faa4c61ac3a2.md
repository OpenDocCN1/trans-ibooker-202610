
involved in request processing as compared to the more powerful annotation approaches of Spring et al. This is a classic performance versus ease-of-use trade-off. You’ll see lots of these in this book.

## Example 5-2. JEE Servlet Example

```
import javax.servlet.http.*;
@WebServlet(
    name = "SkiersServlet",
    urlPatterns = "/skiers"
)
public class SkierServlet extends HttpServlet {
protected void doGet(HttpServletRequest request,
    HttpServletResponse response) {
    // handles requests to /skiers/{skierID}
    try {
        // extract skierID from the request URI (not shown for brevity)
        String skierID = getSkierIDFromRequest(request);
        if(skierID == null) {
            // request was poorly formatted, return error code
            response.setStatus(HttpServletResponse.SC_BAD_REQUEST);
        }
        else {
            // read the skier profile from the database
            Profile profile = GetSkierProfile (skierID);
            // add skier profile as JSON to HTTP response and return
            response.setContentType("application/json");
            response.getWriter().write(gson.toJson(profile));
            response.setStatus(HttpServletResponse.SC_OK);
        }
    } catch(Exception ex) {
        response.setStatus(HttpServletResponse.SC_INTERNAL_SERVER_ERROR);
    }
}
}
```

## State Management

State management is a tricky, nuanced topic. The bottom line is that service implementations that need to scale should avoid storing conversational state. What on earth does that mean?

Let’s start by examining the topic of state management with HTTP.

HTTP is known as stateless protocol. This means each request is executed independently, without any knowledge of the requests that were executed before it from the same client. Statelessness implies that every request needs to be self-contained, with sufficient information provided by the client for the Web server to satisfy the request regardless of previous activity from that client.

The picture is a little more complicated than this simple description portrays, however. For example:

- The underlying socket connection between a client and server is kept open so that the overheads of connection creation are amortized across multiple requests from a client. This is the default behavior for versions HTTP/1 and above.
- HTTP supports cookies, which are known as the HTTP State Management Mechanism⁷. Gives it away really!
- HTTP/2 supports streams, compression, and encryption, all of which require state management.

So, originally HTTP was stateless, but perhaps not anymore? Armed with this confusion (!), I’ll move on to state management in application services APIs that are built on top of HTTP.

When a user or application connects to a service, it will typically send a series of requests to retrieve and update information. *Conversational state* represents any information that is retained between requests such that a subsequent request can assume the service has retained knowledge about the previous interactions. I’ll explore what this means in a simple example.

In the skier service API, a user may request their profile by submitting a GET request to the following URI:

```
GET /skico.com/skiers/768934
```

They may then use their app to modify their city attribute and send a PUT request to a update the resource:

```
PUT /skico.com/skiers/
{
    "username": "Ian123",
    "email":"i.gorton@somewhere.com"
    "city": "Wenatchee"
}
```

As this URI does not identify the skier, the service must know the unique identifier of the resource to update, namely 768934. Hence for this update operation to succeed, the service must have retained conversational state from the previous GET request.

Implementing this approach is relatively straightforward. When the service receives the initial GET request, it creates a session state object that uniquely identifies the client connection. In reality, this is often performed when a user first connects to or logs in to a service. The service can then read the skier profile from the database and utilize the session state object to store conversational state – in our example this would be skierID and likely values associated with the skier profile. When the subsequent PUT request arrives from the client it uses the session state object to look up the skierID associated with this session and uses that to update the skier’s home city.

Services that maintain conversational state are known as stateful services. Stateful services are attractive from a design perspective as they can minimize the number of times a service retrieves data (state) from the database and reduce the amount of data that is passed between clients and the services.

For services with light request loads they make eminent sense and are promoted by many frameworks to make services easy to build and deploy. For example, JEE servlets support session management using the HttpSession object, and similar capabilities are offered by the Session object in ASP.NET.

As you scale the service implementations however, the stateful approach becomes problematic. For a single service instance, you have two problems to consider:

- 1. If you have multiple client sessions all maintaining session state, this will utilize available service memory. The amount of memory utilized will be proportional to the number of clients the service is maintaining state for. If a sudden spike of requests arrives, how can you be certain we will not exhaust available memory and cause the service to fail?
- 2. You also must be mindful about how long to keep session state available. A client may stop sending requests but not cleanly close their connection to allow the state to be reclaimed. All session management approaches support a default session timeout. If you set this to a short time interval, clients may see their state disappear unexpectedly. If you set the session time out period to be too long, you may degrade service performance as it runs low on resources.

In contrast, stateless services do not assume that any conversational state from previous calls has been preserved. The service should not maintain any knowledge from earlier requests, so that each request can be processed individually. This requires the client to provide all the necessary information for the service to process the request and provide a response. This is in fact how the skier API is specified in Table 5-1, namely:

```
PUT /skico.com/skiers/768934
{
  "username": "Ian123",
  "email":"i.gorton@somewhere.com"
  "city": "Wenatchee"
}
```

A sequence diagram illustrating this stateless design is shown in Figure 5-2.

![](img/936a72197720a5158b16541313af61c9_138_0.png)

Figure 5-2. Stateless API Example

Any scalable service will need stateless APIs. The reason why will become clear when I explain horizontal scaling later in this chapter. For now, the most important design implication is that for a service that needs to retain state pertaining to client sessions – the classic shopping cart example – this state must be stored externally to the service. This invariably means an external data store.

## Applications Servers

Application servers are the heart of a scalable application, hosting the business services that comprise an application. Their basic role is to accept requests from clients, apply application logic to the requests, and reply to the client with the request results. Clients may be external or internal, as in other services in the application that require to use the functionality of a specific service.

The technological landscape of application servers is broad and complex, depending on the language you want to use and the specific capabilities that each offers. In Java, the Java Enterprise Edition (JEE)⁸ defines a comprehensive, feature rich standards-based platform for application servers, with multiple different vendor and open source implementations.

In other languages, the Express.js⁹ server supports Node, Flask supports Python¹⁰, and in GoLang a service can be created by incorporating the net/http package. These implementations are much more minimal and lightweight than JEE and are typically classified as Web application frameworks. In Java, the Apache Tomcat server¹¹ is a somewhat equivalent technology. Tomcat is an open source implementation of a subset of the JEE platform, namely the Java Servlet, JavaServer Pages, Java Expression Language and Java WebSocket technologies.

Figure 5-3 depicts a simplified view of the anatomy of Tomcat. Tomcat implements a servlet container, which is an execution environment for application-defined servlets. Servlets are dynamically loaded into this container, which provides lifecycle management and a multithreaded runtime environment.

Requests arrive at the IP address of the server, which is listening for traffic on specific ports. For example, by default Tomcat listens on port 8080 for HTTP requests and 8443 for HTTPS requests. Incoming requests are processed by one or more listener threads. These create a TCP/IP socket connection between the client and server. If network requests arrive at a frequency that cannot be processed by the TCP listener, pending requests are queued up in the Sockets Backlog. The size of the backlog is operating system dependent. In most Linux versions the default is 100.

Once a connection is established, the TCP requests are marshalled by, in this example, a HTTP Connector which generates the HTTP request (HttpServletRequest object as in Figure 5-2) that the servlet can process. The HTTP request is then dispatched to an application container thread to process.
