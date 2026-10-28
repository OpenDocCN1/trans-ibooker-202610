# allintext:username,password filetype:log

Here are some points to know about this query:

+   - **allintext:** matches pages that have both strings, username and password, while **filetype:** checks the extension and only returns results if the extension is .log.

+   - **allintext:** matches all the words after, while **intext:** matches the word directly after it, in the same way that **allintitle:** and **intitle:** work.

+   - If the words listed appear in the title, but not the body text, the page won’t be returned as a result. Therefore, **allintext:** and **intext:** are essentially the same as a regular Google search query.

g. Go back to the GHDB (www.exploit-db.com/google-hacking-database) and in the Quick Search textbox on the right, type **FTP**. A lot of Google dorks related to the File Transfer Protocol (FTP) will appear.

The following query provides a simple way to find FTP directories on either FTP servers or web servers hosting FTP content:

# intitle:"index of" inurl:ftp

This query looks for “index of” in the title bar and “ftp” in the actual URL.

h. Pick five Google dorks, each from a different category, and explain how they can be potentially dangerous. Optionally, click the hyperlinks for the actual Google dorks to see what results get returned. You are not advised to follow any of the links in the search results, as that can be illegal, as explained earlier.

i. Preventing Google dorks can be done with a robots.txt file. Web crawlers are required to adhere to the robots.txt exclusion standard (found at www.robotstxt.org/orig.html), which could specify that web crawlers ignore your website, either partially or fully. While web crawlers from major search engines do honor this specification, cybercriminals have other ways around it and adhere to nothing.

2a–2g