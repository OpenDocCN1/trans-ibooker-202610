As you can imagine, companies that allow clients to perform introspection queries against their GraphQL APIs are making a security trade-off. Information about the various fields and objects that the backend application supports can only aid threat actors and increase their chances of successfully finding vulnerabilities. As such, production-grade implementations will often disable introspection. This means you may be required to test GraphQL applications in production setups that don’t allow introspection queries to be executed. In these situations, figuring out how to properly construct queries may pose a challenge.

This is where Clairvoyance comes to the rescue. This Python-based reconnaissance tool for GraphQL APIs, developed by Nikita Stupin (@_nikitastupin) and Ilya Tsaturov (@itsaturov), allows you to discover schema information when introspection is disabled. It works by abusing a GraphQL feature called field suggestions. Essentially, it reconstructs the underlying schema by sending queries crafted from a dictionary of common English words and observing the server’s responses. We’ll go into more detail about field suggestions and how they help us extract information about GraphQL schemas in Chapter 6.

Let’s go ahead and install Clairvoyance. Open the terminal and enter the following commands:

```
# cd ~
# git clone https://github.com/nikitastupin/clairvoyance.git
# cd clairvoyance
```

We can verify that Clairvoyance is able to run by passing the -h flag to the Clairvoyance script:

```
# python3 -m clairvoyance -h

usage: __main__.py [-h] [-v] [-k] [-i <file>]
[-o <file>] [-d <string>] [-H <header>] -w <file> url

positional arguments:
  url

optional arguments:
  -h, --help      show this help message and exit
```

# InQL

Until recently, not many resources for GraphQL security testing were publicly available, even as GraphQL adoption increased. To fill this gap, the security firm Doyensec developed Introspection GraphQL (InQL).

This security testing tool, based on Python, relies on the introspection query. InQL can export any information it finds about the GraphQL schema to a variety of formats, making the application’s schema easier to read and understand. InQL also can perform other tasks, such as detecting potential DoS conditions.