the exploit sends the HTTP request and calculates how long it takes the server to respond to the expensive query.

In short, if both FORCE_MULTIPLIER and CHAINED_REQUESTS are set to 100, the final array will include 100 queries that each contain 100 duplicated fields. Imagine how expensive such a query might be to process if these two variables were set to 10,000.

> NOTE Since the disclosure, WPGraphQL has made significant security improvements to the plug-in and has addressed this DoS vulnerability.

# Circular Fragments (Agoо)

We discovered a circular fragment vulnerability in May 2022 in a Ruby-based GraphQL server implementation named Agoo. Identified by CVE-2022-30288, the vulnerability stems from the absence of validations checks on incoming queries at the Agoo server level. This failure to validate means the server isn’t spec compliant. It also means that queries sent to an Agoo server can take it down in several ways. Let’s explore how we were able to do this with circular fragments.

As a first step, we wanted to check whether introspection was enabled by default, so we ran the following query:

```
query Introspection {
  __schema {
    directives {
      name
    }
  }
}
```

This query is simple; it returns the name of all the directives in the schema. This is a pretty good query to use when you don’t yet know what operations the GraphQL server supports.

Next, we built a circular query using fragments that reference the query:

```
query CircularFragment {
  __schema {
    ① ...A
  }
}

fragment A on __Schema {
  directives {
    name
  }
  ② ...B
}

fragment B on __Schema {
  ③ ...A
}
```