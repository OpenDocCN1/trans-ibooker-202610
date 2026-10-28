The HTTP `language` tag<sup>38 39</sup> can be used to return a list of supported natural languages for response bodies. The list of valid values should be taken from the IANA Language Subtag Registry.<sup>40</sup>

# profile

The `profile` link relation value<sup>41</sup> can be used to return a list of supported semantic profiles for responses. The contents of this element can be a list of valid URIs that represent semantic vocabularies (see Recipe 4.7 for details).

> **NOTE**
> 
> There is another client preference value that HTTP supports: `accept-ranges`. This allows clients to signal to services that they can support range values for returning responses via the `range` header. You can check out the HTTP specification<sup>42</sup> for details.

The above values should be accessible for client applications via two avenues: 1) the **HTTP OPTIONS method** and, 2) a resource representation returned when following the **“meta” link relation value**<sup>43</sup> (`rel="meta"`). See the example below for details.

Clients can request the service’s supported client preferences and then use values from this response in their request in order to indicate to the service which formats, languages, encodings, etc. they wish the service to use in responses.

Even when there is only one possible response the service supports (e.g. `"language":"en"`), the service should still return the full `meta` resource in order to help clients discover the values for these important parameters.

# Diagram

It is a good idea to support a link on the “Home” resource of your service interface that points to the client preferences `meta` resource. This `meta`