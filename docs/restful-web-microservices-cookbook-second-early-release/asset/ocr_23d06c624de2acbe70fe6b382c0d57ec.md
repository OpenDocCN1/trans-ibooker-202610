The schema identifier may also be passed from server to client in the `Content-Type` header:

```
```http
*** REQUEST
GET /todo/list HTTP/1.1
Host: api.example.org
Accept: application/vnd.hal+json

*** RESPONSE
HTTP/1.1 200 OK
Content-Type
application/vnd.hal+json;schema=urn:example:schema:e-commerce-payment
...
```
```

It is important to point out that adding additional facets (those after the “;”) to the media type string can complicate message format validation. Also, some media type definitions expressly forbid including additional metadata in the media-type identifier string (TK JSON example?).

# Schema Identifier in the Document

It is most common to see the schema identifier provided as part of the message body:

```
```json
{
  "$schema": "https://alps-io.github.io/schemas/alps.json",
  "alps": {
    "version": "1.0",
    "title": "ToDo List",
    "doc": {"value":"A suggested ALPS profile for a ToDo service"},
    "descriptor": [
      {"id":"id", "type":"semantic", "def":"http://schema.org/identifier"},
      {"id":"title", "type":"semantic", "def":"http://schema.org/title"},
      {"id":"completed", "type":"semantic", "def":"http://mamunda.site14.com/alps/def/completed.txt"}
    ]
  }
}
```
```