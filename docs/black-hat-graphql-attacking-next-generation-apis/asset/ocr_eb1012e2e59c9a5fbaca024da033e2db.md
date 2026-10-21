```
username
}
}
```

As expected, the response to this request will return a single user object, its ID, and its username:

```
{
  "data": {
    "users": [
      {
        "id": "1",
        "username": "admin"
      }
    ]
  }
}
```

Arguments can also be passed to nested fields. Consider this query:

```
query {
  users(id: 1) {
    username(capitalize: true)
  }
}
```

The nested username field now has an argument called capitalize. This argument accepts a Boolean value, here set to true. In DVGA, this argument will make GraphQL capitalize the first character of the username field and return it in the response, converting admin into Admin, for example:

```
{
  "data": {
    "users": [
      {
        "username": "Admin"
      }
    ]
  }
}
```

Arguments are unordered, which means that changing their order does not change the logic of the query. In the following example, whether you pass the limit argument or the public argument first doesn’t change the query’s meaning:

```
query {
  pastes(limit: 1, public: true){
    id
  }
}
```