Clarification made, one of the biggest challenges when starting to work with cloud and cloud-native approaches is that applications are *distributed and rely on existing services*. In traditional IT, we still have many monoliths, or in the best case, we work with **service-oriented architecture (SOA)**, which we talked about in *Chapter 4, Reviewing the Historical Architecture Styles.* SOA has proven its value and is certainly still future-proof, but as always, the IT world has evolved, and newer paradigms have emerged.

With cloud and cloud-native applications, we tend to rely more on off-the-shelf frameworks, services, and ecosystems. We *assemble* existing cloud services and add our code on top. Some services come with a lot of built-in functionality. For example, many mainstream artificial intelligence services exist that can be used in applications to perform image recognition, text extraction, and so on. Other services also come with more technical features. For example, **API Management** solutions ship with API throttling, JWT token validation, and many other features out of the box. Moreover, such services are designed with resilience and robustness from the ground up. You should, at best, create a pale copy, should you stubbornly develop it yourself.

To adopt a cloud or cloud-native approach, you need to modernize/refactor the assets or start from a greenfield situation. As a software architect, you must look at how the ecosystem can help you achieve more faster and in a better way. As a software architect, you must offload the *reusability* quality attribute to the provider. You must pay particular attention so that you don't reinvent the wheel.

Although there is probably not a single definition for cloud and cloud-native development, let me share my definitions. A **cloud development approach** looks as follows:

![](bbox=[0.25, 0.62, 0.7, 0.88])

Figure 6.3 – Cloud development approach