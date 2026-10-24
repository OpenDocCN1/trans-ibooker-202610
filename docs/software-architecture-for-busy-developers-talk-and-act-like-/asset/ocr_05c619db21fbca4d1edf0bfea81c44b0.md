I scaled the event publisher for 50 instances. This may or may not work in your own environment, depending on your cluster capacity. If 50 is too much, just reduce it to 5. The goal is to create more events and see how Azure functions scale accordingly. The live metrics screen should show something similar to the following:

![](bbox=[0.19, 0.2, 0.77, 0.46])

Figure 7.19 – Live metrics showing the request rate and duration

No matter how you scaled the event publisher, leave it running for about a minute and then scale the deployment back to zero.

As shown in the preceding screenshot, at peak time, the system was handling about 525 requests per second and most executions took less than 20 milliseconds. We did not have to configure anything; we just let the cloud provider adjust the computing power according to the demand. The following screenshot shows that the system scaled out, up to five instances:

![](bbox=[0.12, 0.65, 0.78, 0.86])

Figure 7.20 – Azure function max instance count