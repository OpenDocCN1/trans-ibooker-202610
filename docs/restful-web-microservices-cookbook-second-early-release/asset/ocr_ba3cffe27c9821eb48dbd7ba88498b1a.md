configuration data is not viable (e.g. security reasons), then creating your own client-side named variable collection in code.

> **WARNING**
> 
> It is important acknowledge that relying on client-side configuration files is not an ideal solution. Changes in service URLs need to be communicated to the client developer so that the client-side information (config or code) can be updated. This presents a possible lag between service changes and clients changes. You can set up client-side monitoring of changes but that’s just adding more work and can only shorten the client application’s “reaction time” to unannounced changes.

It was noted earlier that some URL variables may share the same URL data, but have very different uses. For example, the service URLs for read, update, and delete may be the same (*service.example.org/{id}*). This leads to the next step of client applications keeping the entire collection of request metadata in code, too (URL, method, headers, querystring, and/or body information). See [Recipe 3.10] for more on this option and when to use it.

# Related Recipes

+   - TK (Hypermedia)
+   - Recipe 3.10
+   - see XXX in *Chapter 4*

# 3.2 Code Clients to be HTTP-Aware

Sometimes helper libraries, wrappers, and/or API software developer kits (SDKs) can make it *harder* for API client applications to solve their problems instead of making it easier. For this reason it is important to make sure your API client applications are able to “converse” with services in HTTP (or whatever the target protocol might be).

# Problem