union, interface, and input. In this section, we will define each type and explain what it is used for.

We reference the types defined in DVGA's schema as examples. If you'd like more context, you can use Altair to download the full SDL file for DVGA. To download it, click the Docs link next to the Send Request button and select the ellipsis (...) button to expose the Export SDL option, shown in Figure 3-3.

![](bbox=[0.24, 0.2, 0.9, 0.38])

Figure 3-3: Altair's Export SDL feature

# Objects

Custom object types are groups of one or more fields that define domain- or application-specific objects. Consider the snippet of DVGA's schema in Listing 3-11.

```
type PasteObject {
  id: ID!
  title: String
  content: String
  public: Boolean
  userAgent: String
  ipAddr: String
  ownerId: Int
  burn: Boolean
  owner: OwnerObject
}
```

Listing 3-11: The DVGA PasteObject type

We define a new custom object type, called PasteObject. This object has fields described between curly brackets. You may recognize a few of these fields, as we used them in a GraphQL query earlier in this chapter. Each of these fields uses GraphQL's out-of-the-box scalar types except for the owner field, which is also a custom object type.

If you look at the id field, you'll notice that it contains the exclamation mark (!) character. This means that every Paste object requires an ID, whereas every other field can be null. These required fields are known as non-null wrapping types. Also notice the one-way-link relationship between