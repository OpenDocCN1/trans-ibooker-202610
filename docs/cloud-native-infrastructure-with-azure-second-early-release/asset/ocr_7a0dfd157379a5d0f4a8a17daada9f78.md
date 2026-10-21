+   - Each plug-in will inspect the query, and based on the query, the following can happen:
+     - If the query is processed by the plug-in, an appropriate response will be sent back to the client. The query processing stops right here and no more plug-ins are called.
+     - If the query is not processed by the plug-in, it will call the next plug-in in the chain. If the last plug-in in the chain is also not able to process the query, CoreDNS will return a SERVFAIL to the calling client.
+     - If the query is processed using a fall-through method in which the other plug-ins down the chain are also called to take a look at the query, the keyword `fallthrough` is used to enable it.
+     - If the query is processed by using a hint, the next plug-in in the chain will always be called. However, it will provide a hint that allows it to see the response that will be returned to the client.

Figure 7-1 showcases the steps that occur when CoreDNS processes a query.