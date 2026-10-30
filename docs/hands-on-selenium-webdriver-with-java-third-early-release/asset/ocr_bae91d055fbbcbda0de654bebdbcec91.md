![](bbox=[0.12, 0.09, 0.87, 0.49])

Figure 6-5. New session in a Selenium Grid distributed architecture

# WebDriver Commands

Once the session is established, the Selenium WebDriver API script will continue sending W3C WebDriver commands to control the web browser in the remote node. Figure 6-6 shows how this communication occurs in a distributed Selenium Grid infrastructure following these steps:

+   1. The script/test exchanges W3C WebDriver commands to drive the browser (e.g., open a web page, interact with web elements, etc.) in the current session.
+   2. Further requests to the same browser session use the same session id. The Router recognizes that a browser session is active by reading the Session Map.
+   3. The Router forwards the subsequent commands of the same session directly to the assigned node.