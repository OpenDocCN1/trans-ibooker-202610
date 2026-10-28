```
```json
"id": "read_22s3k36pkn4",
"name": "read",
"href": "http://localhost:8181/22s3k36pkn4",
"rel": "read item",
"title": "Read",
"method": "GET",
"properties": []
},
"id": "22s3k36pkn4",
"givenName": "Mork",
"familyName": "Mockery"
]
}
```
```

# Discussion

Essentially semantic profiles carry the domain elements (person, list, filter, etc.) of the service. You can think of semantic profiles as a method for documenting the service domain.

It is worth pointing out that these semantic profiles do not explain *how* the filter hypermedia control works — just that it might appear in the response. Semantic profiles assume the API consumer application understands that media type independent of the profile. This separation between understanding message formats and understanding the semantics *within* the message is an important feature of well-designed hypermedia services.

When your service doesn’t use a registered media type and instead uses a serialization formation like plain JSON or XML, services generally embed the semantics of the domain into the structure of the message. For example, here’s a possible response for the person service in XML:

```
```xml
<list>
  <home href="..." rel="collection"/>
  <list href="..." rel="collection"/>
  <filter href="..." method="get" rel="collection">
    <givenName></givenName>
    <familyName></familyName>
  </filter>
</list>
```
```