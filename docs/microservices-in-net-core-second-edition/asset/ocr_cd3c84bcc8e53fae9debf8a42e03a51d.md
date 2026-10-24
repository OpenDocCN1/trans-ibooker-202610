# 5.2.3 Implementing commands with HTTP POST or PUT

The code needed in the loyalty program microservice to implement the handling of the two commands—the HTTP POST to register a new user and the HTTP PUT to update one—is similar to the code you saw in chapter 2. You'll start by implementing an action method for the command to register a user. A request to loyalty program to register a new user is shown in the following listing.

# Listing 5.3 Request to register a user named Christian

```
POST /users HTTP/1.1
Host: localhost:5001
Content-Type: application/json
Accept: application/json

{
"name":"Christian",
"loyaltyPoints":0,
"settings":{" interests" : ["whisky", "cycling"] }
}

JSON representation of the user being registered
```

To handle the command for registering a new user, you need to add an MVC controller to the loyalty program by adding a file called UsersController.cs and putting the following code in it.

# Listing 5.4 POST endpoint for registering users

```
using System;
using System.Collections.Generic;
using Microsoft.AspNetCore.Mvc;

[Route("/users")]
public class UsersController : ControllerBase
{
    [HttpPost("")]
    public ActionResult<LoyaltyProgramUser> CreateUser(
        [FromBody] LoyaltyProgramUser user)
    {
        if (user == null)
            return BadRequest();
        var newUser = RegisterUser(user);
        return Created(
            new Uri($"/users/{newUser.Id}", UriKind.Relative),
            newUser);
    }

    private LoyaltyProgramUser RegisterUser(LoyaltyProgramUser user)
    {
        // store the new user to a data store
    }
}

The request must include a LoyaltyProgramUser in the body.
Uses the 201 Created status code for the response
Returns the user in the response for convenience
Adds a location header to the response because this is expected by HTTP for 201 Created responses
```