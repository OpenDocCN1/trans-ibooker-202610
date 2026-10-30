2. We can name and update settings as if we were creating a new VM instance. In previous examples, we set a startup script from gcloud commands but, in this example, let’s set it up from the console. We add this in the Metadata options and use the startup-script-url key and gs://cloudarchitect/startup.sh as our value:

![](bbox=[0.19, 0.2, 0.77, 0.4])

Figure 4.36 – Metadata

3. Click on Instance Groups, and then create a new instance group:

![](bbox=[0.2, 0.48, 0.75, 0.62])

Figure 4.37 – Creating an instance group

Let’s look at the different options that we can specify when creating an instance group.