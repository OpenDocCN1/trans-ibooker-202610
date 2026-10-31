# ZAP's forced browse

DirBuster was a directory brute forcer maintained by OWASP that is now integrated into OWASP ZAP as the forced browse functionality. To use it, you start OWASP-ZAP (in Kali's menu, go to 03 - Web Application Analysis | owasp-zap) and configure the browser to use it as proxy; the same way Burp does passive spidering, ZAP registers all of the URLs you browse and the resources they request from the server. Consequently, you browse to your target and the detected files and directories get recorded in ZAP. Next, right-click on the directory on which you want to do the forced browse and go to Attack | Forced Browse site / Forced Browse directory / Forced Browse directory (and children). The choice between site, directory, or directory and children depends on what you want to scan—site indicates scanning from the root directory of the server, directory means only the selected directory, and directory and children is the selected directory recursively:

![](bbox=[0.13, 0.38, 0.87, 0.74])

After this, select the names list file (dictionary) and click on the Start button. Existing directories and files will possibly show in the same tab: