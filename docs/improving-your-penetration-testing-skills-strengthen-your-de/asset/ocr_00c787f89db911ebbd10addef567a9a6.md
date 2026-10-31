Burp Proxy is a nontransparent proxy, and the first step that you need to take is to bind the proxy to a specific port and IP address and configure the web browser to use the proxy. By default, Burp listens on the 127.0.0.1 loopback address and the 8080 port number:

![](bbox=[0.13, 0.2, 0.87, 0.575])

Make sure that you select a port that is not used by any other application in order to avoid any conflicts. Note the port and binding address and add these to the proxy settings of the browser.

By default, Burp Proxy only intercepts requests from the clients. It does not intercept responses from the server. If required, manually turn it on from the Options tab in Proxy, further down in the Intercept Server Responses section.