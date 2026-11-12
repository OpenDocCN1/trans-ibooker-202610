The default styling of the views doesn’t look too shabby, but they are definitely not the most beautiful designs out there. This, at least, gives you, as a developer, the ability to go back and style the screens with CSS, to prettify them according to your needs. For now, the dull screens are 100% functional and good enough for our purposes.

Open MongoDB Compass and you will see that there is a workitem document in there. View that document and you will see the information that we just added from our ASP.NET Core MVC application:

![](bbox=[0.105, 0.285, 0.89, 0.605])

We now have a working instance of our website. If you plan on extending this application, then this will be the end of the chapter; otherwise, the following section will deal with tearing down the resources that you created.

# # Cleaning up the resources

As mentioned earlier, Microsoft makes their money from Azure based on your usage. In order to avoid incurring costs, you should always clean up (that is, delete) resources that you no longer need. If, like me, you named your Cosmos DB instance bugtracker, then you can return to the Cosmos DB blade (you may have previously pinned this to your dashboard; otherwise, you should search for this) and select the instance.