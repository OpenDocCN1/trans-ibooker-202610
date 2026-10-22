GET  /skico.com/skiers/{skierID}  Get the profile information for a skier, returned in a JSON response payload

PUT  /skico.com/skiers/{skierID}  Update skier profile

DELETE  /skico.com/skiers/{skierID}  Delete a skier’s profile as they didn’t renew their pass!

Additional parameter values can be passed and returned in HTTP request and response bodies respectively. For example, a successful request to:

```
GET /skico.com/skiers/12345
```

will return an HTTP 200 response code and the following results formatted in JSON:

```
{
  "username": "Ian123",
  "email":"i.gorton@somewhere.com"
  "city": "Seattle"
}
```

To change the skier’s city, the client could issue the following PUT request to the same URI along with a request body representing the updated skier profile.