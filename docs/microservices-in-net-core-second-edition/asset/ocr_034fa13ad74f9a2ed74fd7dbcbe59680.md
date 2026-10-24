The response to the preceding request looks like this:

```
HTTP/1.1 201 Created
Content-Type: application/json; charset=utf-8
Location: /users/4
{
  "id": 4,
  "name": "Christian",
  "loyaltyPoints": 0,
  "settings": { "interests": ["whisky", "cycling"]
}
}
```

The status code is 201 Created.

ASP.NET sets the Content-Type.

The Location header points to the newly created resource.

There isn’t much new to notice here, except that we are using the method Created to control the HTTP response. There are many such convenience methods on the ControllerBase base class, and throughout the book we will use them to easily create the HTTP responses we want.

With the action method for the register-user command in place, let’s turn our attention to implementing an action method for the update user command by adding this to the UsersController. (There’s nothing in this code you haven’t seen before.)

# Listing 5.5 PUT endpoint for updating users

```
private static readonly IDictionary<int, LoyaltyProgramUser>
    RegisteredUsers = new Dictionary<int, LoyaltyProgramUser>();

[HttpPut("{userId:int}")]
public LoyaltyProgramUser UpdateUser(
    int userId,
    [FromBody] LoyaltyProgramUser user)
    => RegisteredUsers[userId] = user;
```

Returns the new user and lets ASP.NET turn it into an HTTP response

The actions for the commands are only one side of the collaboration. The other side is the code that sends the commands. Figure 5.2 shows that the API Gateway microservice sends commands to the loyalty program microservice. You won’t build a complete API Gateway microservice here, but in the code download for this chapter, you’ll find a console application that acts as API Gateway with regard to collaborating with the loyalty program. Here, we’ll focus only on the code that sends the commands.

In the API Gateway microservice, you’ll create a class called LoyaltyProgramClient that’s responsible for dealing with communication with the loyalty program microservice. That class encapsulates building and sending HTTP requests to the loyalty program microservice.

The code for sending the register user command takes a user name as input, creates an HTTP POST with a user object in the body, and sends that to the loyalty program microservice. After the response comes back from the loyalty program, we will just return the whole thing as an HttpResponseMessage. That’s an easy solution for