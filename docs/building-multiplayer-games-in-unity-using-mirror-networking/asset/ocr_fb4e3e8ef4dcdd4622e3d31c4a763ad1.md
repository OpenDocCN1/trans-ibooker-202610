That covers it for the server-side code. It’s really easy to use and simple to extend upon.

Then, we have the client-side code.

```
/// <summary>
/// Called on client from StartClient to initialize the Authenticator
/// <para>Client message handlers should be registered in this method.</para>
/// </summary>
public override void OnStartClient()
{
    // register a handler for the authentication response we expect from
    // server.
    NetworkClient.RegisterHandler<AuthResponseMessage>(OnAuthResponseMessage,
    false);
}
```

Again, much like with the server, we need to register a handler, but this time to the AuthResponseMessage so we know to do something when we receive our response from the server. We should register this handler in OnStartClient.

```
/// <summary>
/// Called on client from OnClientAuthenticateInternal when a client needs to
/// authenticate
/// </summary>
/// <param name="conn">Connection of the client.</param>
public override void OnClientAuthenticate(NetworkConnection conn)
{
    AuthRequestMessage authRequestMessage = new AuthRequestMessage();

    NetworkClient.Send(authRequestMessage);
}
```