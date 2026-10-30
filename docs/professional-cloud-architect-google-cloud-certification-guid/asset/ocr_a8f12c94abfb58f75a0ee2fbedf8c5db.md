11. We can now copy this password and use this to RDP to our GCE instance:

# New Windows password

The following is the new Windows password for google3066639_studen.
Copy it and keep it secure. It will not be shown again.

LP*igUh/A(?> &cX

Figure 4.10 – Copying a password

12. If we created a Compute Engine instance with a Linux-based OS, then we simply need to click SSH, as shown in the following screenshot:

![](bbox=[0.12, 0.34, 0.85, 0.455])

Figure 4.11 – SSH

> Exam Tip
> If you do not have an external IP, then a bastion host should be used, also referred to as terminal servers. If you need access to resources that have internal IP addresses only, then bastion hosts are configured with external IP addresses and act as a gateway to your internal resources.

As expected, we can also deploy a GCE instance from the command line. The CLI of choice to create a VM is gcloud via Cloud Shell. We will look at Cloud Shell in more detail in Chapter 16, Google Cloud Management Options, but for now, just understand that it is a command line available directly from the GCP console, and it gives us the ability to quickly create our VM instances:

+   1. To activate it, click on the Cloud Shell icon in the top right-hand corner:

![](bbox=[0.315, 0.73, 0.65, 0.79])

Figure 4.12 – Activating Cloud Shell