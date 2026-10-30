# Run AutoLot.Mvc and AutoLot.Api Together

AutoLot.Mvc depends on AutoLot.Api to be running. This can be done with Visual Studio, the command line, or a combination of the two.

> Note Remember that both AutoLot.Mvc and AutoLot.Api are configured to rebuild the database every time it runs. Make sure to turn at least one of them off or they will conflict. To speed up debugging, turn both off when you are testing functionality that doesn’t change any data.

# Using Visual Studio

You can configure Visual Studio to run multiple projects at the same time. This is accomplished by right-clicking the solution in Solution Explorer, selecting Select Startup Projects, and setting the actions for AutoLot.Api and AutoLot.Mvc to Start, as shown in Figure 31-11.

![](bbox=[0.107, 0.39, 0.896, 0.71])

Figure 31-11. Setting multiple startup projects in Visual Studio

When you press F5 (or click the green run arrow), both projects will launch. There are some intricacies when you do this. The first is that Visual Studio remembers the last profile used to run an app. This means if you used IIS Express to run AutoLot.Api, running both together will run AutoLot.Api using IIS Express, and the port in the service settings will be incorrect. That’s easy enough to fix. Either change the ports in the appsettings.development.json file or run the app with Kestrel before configuring the multiple startup options.