# Denial of Service

+   - Review the API's SDL file for bidirectional relationships.
+   - Test for the following:
+     - Circular queries or mutations
+     - Circular fragments
+     - Field duplication
+     - Alias overloading
+     - Directive overloading
+     - Array-based or alias-based query batching
+     - Object limit overriding in API pagination arguments such as filter, max, limit, and total

# Information Disclosure

+   - Extract the GraphQL schema by using field stuffing when introspection is disabled.
+   - Identify debug errors in query responses by sending malformed queries.
+   - Identify query tracing in GraphQL responses.
+   - Test for any PII submitted using queries over the GET method.

# Authentication and Authorization

+   - Test access to the following:
+     - The API without authentication headers
+     - Restricted fields by using alternate paths
+     - The API by using both the GET and POST methods
+   - Test signature validation in JSON Web Token (JWT).
+   - Attempt to brute-force mutations or queries that accept secrets, such as tokens or passwords, using the following:
+     - Alias-based query batching
+     - Array-based query batching
+     - CrackQL
+     - Burp Suite