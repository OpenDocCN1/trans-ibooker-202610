# Unions

A union is a type that returns one of many object types. A client can leverage unions to send a single request to a GraphQL server and get a list of objects. Consider Listing 3-14, which shows a query using a search feature in DVGA. This feature allows a client to search for a keyword that returns multiple Users and Paste objects:

```
query {
  search(keyword: "p") {
    ... on UserObject {
      username
    }
    ... on PasteObject {
      title
      content
    }
  }
}
```

Listing 3-14: The DVGA search feature

This search feature empowers clients to find both pastes and users that match the keyword with just a single request. Pretty neat! The response to the query can be seen in the following code. It returns a list of matching paste and user fields that have the letter p in either their username or title:

```
{
  "data": {
    "search": [
      {
        "title": "This is my first paste",
        "content": "What does your room look like?"
      },
      {
        "id": "2",
        "username": "operator"
      }
    ]
  }
}
```

To accept and resolve a request like this, a schema can use a union type. In Listing 3-15, we define a union named SearchResults.

```
union SearchResults = UserObject | PasteObject

type UserObject {
  id: ID!
  username: String!
}

type PasteObject {
  id: ID!
```