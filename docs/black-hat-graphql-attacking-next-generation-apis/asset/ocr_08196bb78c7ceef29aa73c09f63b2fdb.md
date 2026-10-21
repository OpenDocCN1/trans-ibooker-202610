```
kind
fields {
  name
  type {
    name
    kind
  }
}
```

Listing 3-20: An introspection query for discovering fields within an object of interest

In this case, we decided to dive deeper into the PasteObject type. You will notice that we are selecting not just the name of the type but also its kind, which returns the __TypeKind introspection type for the object. We’re also selecting all of the PasteObject fields and their names, types, and kinds. Let’s take a look at the response:

```
"__type": {
  "name": "PasteObject",
  "kind": "OBJECT",
  "fields": [
    {
      "name": "id",
      "type": {
        "name": null,
        "kind": "NON_NULL"
      }
    },
    {
      "name": "title",
      "type": {
        "name": "String",
        "kind": "SCALAR"
      }
    },
    --snip--
    {
      "name": "content",
      "type": {
        "name": "String",
        "kind": "SCALAR"
      }
    },
    {
      "name": "owner",
      "type": {
        "name": "OwnerObject",
        "kind": "OBJECT"
      }
    }
  ]
}
```