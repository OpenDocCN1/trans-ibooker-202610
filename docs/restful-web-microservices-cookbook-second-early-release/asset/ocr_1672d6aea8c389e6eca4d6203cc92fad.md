shopping.example.com/home"). Then, when the initial location fails to respond, you can update the runtime code to point to the alternate service. This has some implications for state management — see Recipe 4.15 for details.

# Dynamic Fallback Options

You can also write your API aggregator to be able to enlist a service registry to find another available alternative for your needed functionality. This is, essentially, a runtime service lookup similar to the runtime Domain Name Services (DNS) that locates machines. Check out [Link to Come] for more details. Also, we’ll cover this registry idea in greater detail in [Link to Come].

# Queuing Requests for Later Replay

If you can’t gain a connection to either your initial service or an expected replacement service (see above), you may be able to simply “hold onto” the request in a queue and replay it later, when the currently failing service is once again available. This requires setting up and managing a local queue and some code to process that queue’s content. Typically, the initial response in this case is an HTTP `202 Accepted` with a response body that describes details on how the API consumer can monitor the handling of the delayed request (see [Link to Come]). Of course, you’ll need to include this possibility in your documented interface design so that API consumers can be prepared for this kind of response.

# Give Up

The final option is to just stop processing requests, tell the API consumer that is calling your API that you are “unable to process the request at this time” and return an HTTP 500 error. This is the safest and the least functional option. In this case, if possible, you should return a response body that indicates some estimated wait time before the API consumer can try this request again. Keep in mind that *your* `500 Internal Service Error` might trigger the API consumer