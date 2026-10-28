# Universal Print – required URLs

A Windows endpoint that is being used with Universal Print needs to have an internet connection, with access to the following internet (TCP/IP – 443) endpoints:

+   - *.print.microsoft.com
+   - *.microsoftonline.com
+   - *.azure.com
+   - *.msftauth.net
+   - go.microsoft.com
+   - aka.ms

# Delivery Optimization

For peer-to-peer traffic, Delivery Optimization uses 7680 for TCP/IP or 3544 for NAT traversal (optionally Teredo). For client-service communication, it uses HTTP or HTTPS over port 80/443.

Delivery Optimization will help with band-wide consumption on Windows endpoints:

+   - *.do.dsp.mp.microsoft.com
+   - *.dl.delivery.mp.microsoft.com
+   - *.emdl.ws.microsoft.com

# Summary

In this chapter, you've learned about the different supported endpoint scenarios within MEM and what the requirements are to use the service.

In the next chapter, we're going to take a deeper dive into how to deploy Windows 10 Enterprise to your endpoints. We're also going to talk about Autopilot and how this deployment service can deliver out-of-the-box configurations for your physical endpoints!

# Questions

1. What is the most important set of privileges to have in order to configure MEM?
   A. Global Admin
   B. Azure Owner
   C. Intune Administrator