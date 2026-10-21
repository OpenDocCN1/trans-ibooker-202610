# The REST Example

In applications that use REST APIs, we define specific endpoints, or routes, at which clients can perform actions such as reading or writing data using specific HTTP methods (such as GET or POST). Table 1-2 defines two REST API endpoints for two purposes: one for getting a list of users and another to get information about a user's login history.

Table 1-2: REST API Definitions

| HTTP method | API endpoint | Endpoint description |
|---|---|---|
| GET | /rest/v1/users | Returns a list of all available users and their information |
| GET | /rest/v1/history/<user_id> | Returns a list of the login timestamps for a given user |

When a systems administrator wants to view the Users Administration page, their web client, such as a web browser, will need to obtain information about all available users through the web application’s API. To retrieve this data using the API endpoints in Table 1-2, the web browser would need to send a GET request to /rest/v1/users. Listing 1-5 shows this request and its response.

```
# curl http://lab.blackhatgraphql.com/rest/v1/users
[
  {
    "email": "david@example.com",
    "first_name": "David",
    "id": 1,
    "last_name": "Smith",
    "state": "disabled",
    "username": "dsmith"
  },
  {
    "email": "chris@example.com",
    "first_name": "Chris",
    "id": 2,
    "last_name": "Larry",
    "state": "enabled",
    "username": "clarry"
  }
]
```

Listing 1-5: GET request to /rest/v1/users that lists all system users

As you can see, this request returns the list of all users in JavaScript Object Notation (JSON) format, along with their emails, names, IDs, and account states.

But what if the system administrator wants to retrieve only certain information about users, such as their email addresses, without returning any other information? Using the API definitions in Table 1-2, this wouldn’t