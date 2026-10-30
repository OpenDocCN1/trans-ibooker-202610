among many others. As you can see on Figure 5-1, the WebDriver API provides a set of Java classes to define these capabilities. The `Capabilities` interface is at the top of this hierarchy. Internally, the capabilities interface handles data using key/value pairs that encapsulate specific aspects of a browser. Then, different Java classes implement this interface to specify capabilities for web browsers (Chrome, Edge, Firefox, etc.). Table 5-1 summarizes the main classes of the `Capabilities` hierarchy and their corresponding target browsers.

![](bbox=[0.145, 0.297, 0.856, 0.732])

*Figure 5-1. Capabilities hierarchy*