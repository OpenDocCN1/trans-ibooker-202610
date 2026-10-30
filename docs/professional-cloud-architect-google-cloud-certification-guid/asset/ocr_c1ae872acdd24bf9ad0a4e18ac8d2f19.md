# Pricing

The price of Cloud Functions consists of multiple factors. These include the number of **Invocations**, **Compute time**, and network rate (**Networking**). These are shown in the following diagram:

![](bbox=[0.22, 0.22, 0.78, 0.28])

Figure 9.16 – Cloud Functions pricing

Remember that there is a monthly free usage tier that you can play around with without generating any cost. At the time of writing this book, it consists of 2 million invocations, 1 million seconds of compute time, and 5 GB of egress network traffic. Enjoy it!

# Summary

In this chapter, we talked about Cloud Functions and several use cases where it works perfectly. We talked about two types of functions, namely HTTP and background functions, and also understood that functions can be executed via a particular event or an HTTP request. Finally, we looked at how a function can be deployed both with the Google Cloud Console and with the gcloud command. For the exam, it is important to understand the use cases of Cloud Functions and when using them could be to our advantage.

With this chapter, we have concluded with the Google Compute options. In the next chapter, we will have a look at networking.

# Further reading

For more information regarding the topics that were covered in this chapter, take a look at the following resources:

+   - **Cloud Functions behind the scenes**: https://cloud.google.com/functions/docs/concepts/exec
+   - **Cloud Functions and VPC**: https://cloud.google.com/functions/docs/connecting-vpc
+   - **Local Emulator**: https://cloud.google.com/functions/docs/emulator