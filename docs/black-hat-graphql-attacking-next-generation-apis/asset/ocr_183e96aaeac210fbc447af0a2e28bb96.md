As you can see, our paste shows up on the web page. This is a good opportunity to start testing the createPaste field with inputs such as JavaScript code to see whether the data is safely rendered in the web interface.

Go ahead and create a new paste using the mutation query shown in Listing 8-7.

```
mutation {
  createPaste(title:"XSS", content:"<script>alert(\"XSS\")</script>") {
    paste {
      title
      content
    }
  }
}
```

Listing 8-7: Injecting an XSS payload using the createPaste mutation

This mutation creates a new paste that includes a JavaScript <script> tag in the content argument. If the application is vulnerable to XSS, this code will get rendered in the browser and an alert message box will pop up with the message XSS. After sending this mutation query, head over to the Public Pastes page. You should be greeted with a pop-up message, as shown in Figure 8-15.

![](bbox=[0.24, 0.47, 0.905, 0.72])

Figure 8-15: An XSS payload triggered via a malicious mutation

Let’s walk through what happened here. We first created a new paste using createPaste, supplying a malicious JavaScript payload to the mutation’s content argument. The API then stored the new paste in the database. Because our client is using a GraphQL subscription operation over the WebSocket protocol, and since subscriptions are real time, we immediately see the new paste we created containing the malicious JavaScript code. This is an example of a stored XSS vulnerability.