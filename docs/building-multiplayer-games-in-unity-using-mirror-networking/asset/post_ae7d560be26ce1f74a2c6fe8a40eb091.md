
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

- **PlayerSpawnMethod** - Specifies if Mirror Networking should spawn the player at random NetworkStart locations or in a round-robin style. For this to work, you need to have game objects in the scene with a NetworkStart component.

## Registered Spawnable Prefabs

The registered spawnable prefabs is a list of manually registered prefabs that are allowed to be spawned during runtime. These can be assigned automatically too using a different process that we’ll cover later in this book. For now, it’s important to note that any prefab not in the scene file needs to be registered in this list if you intend to instantiate and spawn the prefab during the game. We’ll cover spawning networked objects later in the book.

## Network Identities

Network identities are the heart of Mirror Networking; they’re used to identify objects over the network. If something needs to be networked, it needs a network identity. More importantly, a network identity cannot be a child of another networking identity. This is important to know, as future versions of Mirror will outright stop this from happening. So, it’s best to keep any networking logic on the root of your networked prefab, along with its network identity. It’s also important to note that any network identities that exist in the scene, before a server exists and is connected to, will be disabled by Unity. This is important because executing logic in Awake will cause the logic to execute, then have the object spawned, potentially destroying some logic that was initialized in Awake and not calling awake again. So, it’s best to avoid Awake for any components attached to a network object. Instead, have your class derive from NetworkBehaviour, and call public void override OnStartClient. We’ll discuss network behaviors later in the book when we go into more detail on the coding behind creating multiplayer games using Mirror Networking.

## Summary

In this chapter, we learned about Mirror Networking and different networking architectures, transport layers, as well as the server, client, and host. We also learned about the network manager and how to configure a multiplayer environment. We then dove into authentication, player objects, and registering spawnable prefabs.

In the next chapter, we’ll discuss the chain of command and get a solid understanding of authority and how it impacts the way that you build a multiplayer game using Mirror.

## EXERCISE 1 – CREATING A CUSTOM OAUTH2 AUTHENTICATOR USING GOOGLE

So, we’re looking to build a custom authenticator using Google login. The standard route to do this would be to use Google’s SDK; however, if you’re looking to develop something that also supports desktop, you’ll encounter a certain hurdle. As of writing, Google’s SDK does not support native unity desktop applications.

In order to build a login flow for the desktop build target, you would need to build an entirely custom login flow using the player’s browser, and then intercept the web response.

Since there are no guides available for doing this in Unity, let alone Mirror, I’ll show you how to create that custom login flow. It’s worth noting that the same concept could apply to Facebook. However, and this is a big one, Facebook’s redirects need to redirect to an SSL-encrypted endpoint through https. This means that redirecting to client local hosts is not feasible. You will need to host an authentication site to receive any redirects if you plan to use this route.

Let’s get started. There are a few things that we’ll need to set up first before we begin:

Step 1 – To begin, we’ll need to register a new app through Google’s API developer portal. In order to do this, you’ll need to navigate to https://console.cloud.google.com/apis/dashboard and log in. Add an app, and follow the prompts.

Step 2 – Next, you’ll want to create an API oAuth 2 client ID to use; you can do this by going to https://console.cloud.google.com/apis/credentials and ensuring that your project is selected in the top bar (Figure 3-9).

So why are there so many models? The sad truth about that is the direct consequence of some of your players, the cheaters. Cheaters will always be around and will always jump at the opportunity of playing by their own rules. Cheaters cheat for their gain or simply the gratification of ruining another player’s experience.

Game server architecture helps tackle the overall design of your networking to mitigate cheating or, in the case where you don’t care if there are cheaters, like a local co-op game, to improve the player experience. Thus, architecture is a design philosophy that you should think through before even writing your first line of network code, keeping in mind time versus effort versus reward.

## Input vs. State-Based Architecture

When we build a multiplayer game, what exactly are we replicating over the network? One might think this would be a simple question to ask, but it can depend entirely on your game or product, the genre, and what needs the architecture should fulfill.

That brings me to the topic of input- or state-based replication. Let’s first cover what exactly I mean when I say that. When you use an input-based replication system, we send the player’s inputs through the network, such as a “PlayerPressedForwardInput()” command.

On the other hand, if we’re using a state-based architecture, we send the positional values over the network instead of sending the movement input itself. Each of these approaches has its place in networked game server architecture. We can use input replication, or state-based replication, or a combination of both, in our game server architecture. Each of these approaches has its place in networked game server architecture. Our choice depends on whether or not our network simulation is deterministic or not.

## Input-Based Replication – Pros and Cons

Typically, the pros and cons of these different forms of replication depend on the architecture itself since some of these may change. However, in general, input-based replication has the following pros and cons:

Pros

- **Typically has significantly lower bandwidth usage** - This is since you’re only invoking movement and interaction commands. Furthermore, in a deterministic environment, you won’t even need to replicate any values.
