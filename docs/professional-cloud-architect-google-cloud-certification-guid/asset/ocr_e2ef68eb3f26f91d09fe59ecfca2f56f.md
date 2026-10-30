If we click on this method, it will bring us to a page that allows us to request a VM instance via the API. The first thing that we should notice is the menu on the right-hand side, where we can select OAuth 2.0 as our authentication method. This allows us to authorize the API call using the same user we used to log into the GCP console. There is some mandatory information to populate, and this is specified in red:

+   - **project**: This is the ID of your GCP project.
+   - **zone**: This is the zone where you would like the VM to reside.

The following screenshot shows an example of these fields populated:

![](bbox=[0.31, 0.29, 0.7, 0.51])

Figure 16.3 – Try this API

We also need to populate the request body with information about our request. Let's look at what is required in the following example. We are required to provide information regarding the machine's type, the machine's name, and the disk and image type:

```
{
  "machineType": "zones/us-central1-f/machineTypes/n1-standard-1",
  "name": "test001",
  "disks": [
    {
      "initializeParams": {
        "sourceImage": "projects/debian-cloud/global/images/family/debian-9"
      },
      "boot": true,
```