```
}
    catch(ex) {
        alert(ex);
    }
}
}
```

Notice, in the above example, that the client can “converse” in three message formats: Collection+JSON, SIREN, and HAL. If the service returns anything else, the application sends it to the dump() routine for additional processing.

# Discussion

Organizing your client code to be driven by message formats works best when services support highly-structured formats like HTML, HAL, SIREN, Collection+JSON, etc. When services use unstructured response formats like XML and JSON, you’ll need additional metadata in order to support message-driven client code. In that case, schema documents like XML Schema 13 and JSON Schema: 14 can be handy. See Recipe 3.7 for more on using schema-documents on the client-side.

Recognizing the incoming message format is just the first step. You also need to be able to parse and often translate that message for internal use. For Web browsers, you can build a single Javascript library that converts incoming JSON formats (HAL, SIREN, CollectionJSON) into HTML for rendering. Below are the high-level methods for converting JSON-based API messages into HTML:

```
// collection+JSON-->HTML
function parse(collection) {
    var elm;

    g.cj = collection;

    title();
    content();
    links();
    items();
    queries();
    template();
```