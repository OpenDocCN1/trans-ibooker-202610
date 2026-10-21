```
{
    "title": "My Title!"
},
"queryTwo": [
    {
        "title": "Testing Testing"
    }
]
}
```

> NOTE While aliases can typically contain alphanumeric characters, most GraphQL servers will return a syntax error when aliases contain special characters.

So far, aliases look pretty innocent. Rest assured that we can weaponize them. In Chapter 5, we will teach you how to leverage aliases for a variety of DoS attacks, and in Chapter 7, we’ll use them to defeat authentication controls.

# Fragments

Fragments allow clients to reuse the same set of fields in a GraphQL query for readability and to avoid field repetition. Instead of repeating the fields, you can define a fragment once and use it whenever you need that particular set of fields.

Fragments are defined using the fragment keyword, followed by any name you desire, and declared using the on keyword on an object type name:

```
fragment CommonFields on PasteObject {
  title
  content
}
```

In this example, we define a fragment named CommonFields. Using the on keyword, we declare that this fragment is related to the PasteObject, which can give us access to fields that you are already familiar with by now, such as title and content. Listing 3-6 shows how to use this fragment in a query:

```
query {
  pastes {
    ...CommonFields
  }
}

fragment CommonFields on PasteObject {
  title
  content
}
```

Listing 3-6: Defining the CommonFields fragment and using it in a query