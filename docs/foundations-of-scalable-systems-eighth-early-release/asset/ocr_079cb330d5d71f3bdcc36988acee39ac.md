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