# Cloud Storage FUSE

Cloud Storage cannot be mounted to a Google Compute Engine instance. However, if this is something that you would like to explore, GCP currently offers third-party integration using an open source FUSE adapter that will allow you to mount a storage bucket as a filesystem on Linux. This is available free of charge and is not officially supported; however, normal Cloud Storage charges are applicable.

# Creating and using a bucket

Now that we have a good understanding of storage classes, let’s take a look at how to create a bucket. We will also learn how to copy files into our bucket and change the storage policy of an object.

We have two options to create a bucket – either through a console or using gsutil. We will learn more about gsutil in Chapter 16, Google Cloud Management Options, but for the context of this chapter, it’s enough for you to know that it is a dedicated command-line tool that will make RESTful API calls to your Cloud Storage service. Let’s look at how to create a bucket from the console by going through the following steps:

+   1. Browse to STORAGE from the navigation menu. Then, select Cloud Storage | Browser:

![](bbox=[0.275, 0.462, 0.725, 0.638])

Figure 11.3 – Navigating to Cloud Storage