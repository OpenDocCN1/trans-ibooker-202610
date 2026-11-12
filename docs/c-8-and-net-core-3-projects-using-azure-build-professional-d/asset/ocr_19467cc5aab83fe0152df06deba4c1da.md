![](bbox=[0.106, 0.106, 0.893, 0.574])

It's very likely that you'll have a different set of services, but the principle is the same. These applications all run in the background and do something. Every single one of these services that are marked as running is consuming resources from your computer, so, when you install a service, you should consider whether you need it to be running all the time (otherwise, simply disable it).

> Disabling some Windows Services can cause your machine to behave differently, or even stop some functionality, so be careful if you intend to disable services that were installed when you arrived.

If we're writing a service, this is true twice over: people will notice very quickly if it is your application that has slowed their machine down, so performance is a huge consideration here.