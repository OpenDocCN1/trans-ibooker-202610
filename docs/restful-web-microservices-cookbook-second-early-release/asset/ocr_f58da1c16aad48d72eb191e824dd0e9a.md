The trick of converting `application/x-www-form-urlencoded` to JSON works well as long as the resulting JSON is treated as simple name/value pairs. Most FORMs-formatted messages follow this pattern but there are cases where service get creative with FORM field names that imply a nested hierarchy. For example:

```
user.familyName=mamund&user.givenName=ramund&user.sms=+1234567890
```

implies a server-side message that looks like this:

```
{
  "user": {
    "familyName": "mamund",
    "givenName": "ramund",
    "sms": "+12345678901"
  }
}
```

As the service get more clever with naming patterns, the suggested trick of converting FORM strings to JSON documents get less reliable. To avoid the problem, it may be better to write a code-based validator instead.

In the best case, services will supply client application developers with properly-written (and up-to-date) schema documents to use at runtime. If the target service does not do this, application developers should review the human-readable documentation for the service API and craft their own schema documents for use at runtime. These documents should be posted somewhere online (possibly behind a secured URL) that is reachable by client applications. Care must also be taken to keep these schema documents up-to-date as the service changes over time.

> **WARNING**
> 
> When in doubt, write your own custom message checking code directly to avoid any problems with poorly-written or inadequately-maintained schema documents.