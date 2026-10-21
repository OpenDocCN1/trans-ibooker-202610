the server. API consumers aren’t always browsers; machines, such as other servers on the network, can be GraphQL API consumers too.

Unlike other API formats, GraphQL allows an API consumer to request specific data from an application’s server without also receiving unnecessary information. Contrast this approach with traditional REST API architectures, which provide a fixed data structure and then rely on the clients to filter out any unnecessary information they don’t need. We’ll compare the REST and GraphQL API response structures in “GraphQL APIs vs. REST APIs” on page 9 to illustrate the differences between the two.

From a security perspective, GraphQL’s design provides an advantage. Because GraphQL doesn’t return data that the client doesn’t explicitly ask for, its use reduces opportunities for information disclosure issues. Returning more data than a client needs could lead to the unintentional exposure of sensitive data, such as personally identifiable information (PII), which could cause many other problems, especially for companies operating under heavy regulatory rules. However, as you’ll soon see, GraphQL also has security weaknesses that we, as hackers, can exploit.

# Origins

Facebook developed GraphQL in 2012 and used it for a few years in its production environments before releasing it as open source software in 2015. That year, Facebook also developed and released the GraphQL specification and a reference implementation named GraphQL.js (https://github.com/graphql/graphql-js), built using JavaScript.

GraphQL is now maintained by the GraphQL Foundation (https://graphql.org/foundation/), an organization founded by global technology companies. The foundation funds mentorship and project grants for GraphQL maintainers, manages policies of the GraphQL trademark, provides legal support for projects, and supports community-related infrastructure.

# Use Cases

Just about any application and device can use GraphQL. Companies may consider using it if their clients often request a lot of information at the same time, which would otherwise require making many REST API calls. Using GraphQL could reduce bandwidth usage and improve client performance.

For instance, imagine a website dashboard that consolidates information about the weather from multiple third-party weather websites and that is consumed by mobile clients on slow data networks. If the dashboard had to make a bunch of calls to the various weather networks and filter through the data, this wouldn’t be an optimized process. GraphQL allows the fetching of complex data structures in a single request, significantly reducing the required number of client and server round trips. You’ll learn more about this bandwidth-optimization design later in this chapter.

Today, many large-scale companies, such as Facebook, Atlassian, GitHub, and GitLab, use GraphQL, serving hundreds of millions of customers on