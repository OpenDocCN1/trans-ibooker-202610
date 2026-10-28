decides which media type to use. This is sometimes called “server-driven content negotiation”.

Clients indicate their response preferences using the `Accept` header in requests and servers indicate their final section using the `Content-Type` header in responses (see example below).

In PCN, clients *advise* servers of their preference and the servers make the final determination of the response format. It should be noted that it is possible that the service will respond on a format that was *not included* in the client’s `Accept` header. Using the above example, if the service does not support any JSON formats, that service might response with `text/plain` or `application/HTML`.

# Reactive Content Negotiation (RCN)

There is an alternative to proactive content negotiation by the service. That alternative is called reactive content negotiation (RCN). In RCN, the service sends the client a list of possible representation formats and the client is expected to select the preferred format from the list and make the request a second time, specifying which response format has been selected. This puts the power of selection squarely in the hands of the client application but also means an additional “round trip” is made each time.

RCN works well when there are lots of variables (language, message format, encoding scheme, etc.) but the details of how server and client interact in order to select the preferred response format are not spelled out in the HTTP specification. A common approach is for servers to respond with HTTP status code `300 Multiple Choices` with a response body that lists options for clients to review. The `300 Multiple Choices` response⁴⁹ often carries a series of `Link` headers, possibly also a `Location` header in case the client wants to just redirect automatically.

# Example

Both Proactive and Reactive content negotiation are supported by HTTP. Below are some examples.