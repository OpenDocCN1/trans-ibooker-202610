not about the underlying service behind the API.

When creating a new problem details `type`, you should document three things:

+   - A `type` URI (e.g. *http://api.example.org/problems/insufficient-funds*)
+   - A `title` (e.g. “Your account has insufficient funds for this transaction.”)
+   - A `status` code that is appropriate for this problem (e.g. 403)

Whenever possible, it is a good idea to limit the number of new problem details `type`s you define and to create ones that are general enough to be reusable. You can also customize the problem representations using the `detail` (text) and `instance` (URL) elements of the message.

It is also possible to indicate the use of a `Retry-After` header with a problem details response. This makes it possible indicate retry details to API consumers when needed.

# Related

+   - Chapter 2
+   - Chapter 3
+   - Chapter 4
+   - [Link to Come]

# 4.13 Increasing Throughput with Client-Supplied Identifiers

It is common practice to expect servers to generate and return identifiers for any resources created by client applications. This simplifies the process of ensuring unique resource URLs but it has drawbacks. This is especially true when multi-step processes are involved (create A, get unique ID from A, use that to create B, etc.).