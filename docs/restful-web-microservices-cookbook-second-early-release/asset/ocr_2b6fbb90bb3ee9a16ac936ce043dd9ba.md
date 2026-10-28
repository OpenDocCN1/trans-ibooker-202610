+   - URL: /onboarding/contact with schema { contact: {...} }

+   - URL: /onboarding/agreement with schema { agreement: {...} }

+   - URL: /onboarding/review with schema { review: {...} }

For the service, there are four defined steps that are executed in sequence. That sequence is often outlined, not in the service code, but in the service documentation. That means it is up to the client application to convert the human-speak in the documentation into machine-speak in the code. And it usually looks something like this:

```
function onboardCustomer(customer, contact, agreement, review) {
  http.send("/onboarding/customer", "POST", customer);
  http.send("/onboarding/contact", "POST", contact);
  http.send("/onboarding/agreement", "POST", agreement);
  http.send("/onboarding/review", "POST", review);
  return "200 OK";
}
```

The first challenge in this example is that the static binding means any change to service workflow (e.g. add a creditCheck step) will mean the client app is “broken”. A better approach is the tell the client what work needs to be done and provide the client application the ability chose and execute steps as they appear. We can use hypermedia in responses to solve that problem:

```
function onboardCustomer() {
  results = http.read("/onboarding/work-in-progress", "GET");
  while(results.actions) {
    var action = results.actions.pop();
    http.send(action.url, action.method, map(action.parameters,local.data));
  }
  return "200 OK";
}
```

In the simplified example above, the client “asks” the service for a list of actions to take and then performs those actions using the runtime metadata (see “Runtime Resolution with Metadata”) in the message. The client