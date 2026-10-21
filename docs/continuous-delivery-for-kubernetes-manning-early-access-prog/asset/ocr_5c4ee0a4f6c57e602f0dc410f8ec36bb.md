As soon as the HTTP Header is present Knative Serving will route the incoming request to the debug tag.

![](bbox=[0.18, 0.18, 0.7, 0.44])

Figure 5.12 A/B testing with Http Header-based routing to Debug version

![](bbox=[0.18, 0.48, 0.9, 0.7])

Figure 5.13 Using ModHeader Chrome extension to set custom HTTP Headers

Because the Header-based routing experimental feature also uses tags to route traffic. If you still have the `color` tag created, you can test traffic is being routed by changing the `Knative-Serving-Tag` header value to `color` and see if the correct page comes back.

Both tag and header-based routing are designed to ensure that all requests will be routed to the same revision if you hit a specific URL (created for the tag) or if one particular Header is present. Finally, let’s take a look at how to do Blue/Green Deployments with Knative Serving.