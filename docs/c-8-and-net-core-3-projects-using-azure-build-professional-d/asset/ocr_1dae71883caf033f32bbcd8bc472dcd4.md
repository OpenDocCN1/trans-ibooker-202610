# # Client application

Before we can introduce IdentityServer to validate that the correct people and applications can access the API, we need to have an application that we can say can legitimately access the API; otherwise, the best thing we could do is to simply prevent access to the API altogether. Our client application will be built using **Universal Windows Platform (UWP)**.

> UWP is Microsoft's preferred method for building Desktop applications. WPF and WinForms are still supported (and if you’ve read the previous chapters, you’ll see that they’re getting a new lease of life). However, for new applications, it is recommended that you use UWP. In fact, XAML Islands are a way to bridge the gap between the old and the new.

Our application will be very simple: we just want a single screen with a lookup for the stock level and an option to update the stock levels.

> I’ve never claimed to be a UX designer, so if you feel you could design the screen better, that’s probably because you could (and there is nothing in the functionality that will be altered if the layout of the screen is changed)!

Let’s create a new project inside our solution:

![](bbox=[0.153, 0.52, 0.847, 0.889])