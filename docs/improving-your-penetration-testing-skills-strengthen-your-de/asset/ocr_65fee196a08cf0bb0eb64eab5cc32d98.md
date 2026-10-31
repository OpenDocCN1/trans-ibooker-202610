+   - Wapiti
+   - OWASP-ZAP

# Nikto

A long-time classic, Nikto is perhaps the most widely used and well-known web vulnerability scanner in the world. Even though its scanning operation is not very deep and its findings are somewhat generic (they are, by and large, related to outdated software versions, the use of vulnerable components, or misconfigurations detected by analyzing the response headers), Nikto is still a very useful tool because of its extensive set of tests and due to its low likelihood of breaking things.

Nikto is a command-line tool. In the following screenshot, nikto is used with the parameters -h for the host or URL that we want to scan and -o to specify the output file. The extension of the file determines the format of the report. Other common formats are .csv (for comma separated file) and .txt (for text files):

For more details and other options to use with nikto, run it with the -H option, for full help.

![](bbox=[0.175, 0.485, 0.825, 0.77])