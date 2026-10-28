Microservices make it possible to better-target your service changes and to limit unwanted side-effects in running systems. That’s a key element in our recipes — improved targeting and limited side-effects even when they span multiple machines hosted in various locations around the world.

# Alan Kay’s Extreme Late Binding

Another important aspect of creating reliable, resilient microservices that can “live on the web” comes from U.S. computer scientist, Alan Kay¹⁰. He is often credited with popularizing the notion of “object-oriented” programming in the 1990s.

> ### ALAN KAY ON OOP
> 
> When explaining his view of object-oriented programming (OOP) on an email list in 2003, Kay stated: “OOP to me means only messaging, local retention and protection and hiding of state-process, and extreme late-binding of all things.”¹¹

In 2019, Curtis Poe wrote a blog post¹² exploring Kay’s explanation of OOP and, among other things, Poe pointed out: “Extreme late-binding is important because Kay argues that it permits you to not commit too early to the *one true way* of solving an issue (and thus makes it easier to change those decisions), but can also allow you to build systems that you can change *while they are still running*.” (emphasis Poe’s)

> ### TIP
> 
> For a more direct exploration of the connections between Roy Fielding’s REST and Alan Kay’s OOP, see my 2015 article “The Vision of Kay and Fielding: Growable Systems that Last for Decades”

Just like Kay’s view of programming using OOP, The web — the Internet itself — is **always running**. And any services we install on a machine attached to the Internet is actually changing the system while it is running. That’s what we need to keep in mind when we are creating our services for the web.