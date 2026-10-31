To take advantage of Shodan's advanced search features, one needs to first create an account. Free accounts yield a limited number of results, and some options are restricted though still very useful. Shodan can be used to find the following:

+   - Servers exposed to the internet belonging to some domain can be found like this:
+     ```
+     hostname:example.com
+     ```
+   - Specific types of devices, such as CCTV cameras or **Industrial Control Systems (ICS)**, can be found by specifying the Server parameter:
+     ```
+     Server: SQ-WEBCAM
+     ```
+   - Specific open ports or services can be found, for example, web servers using common ports:
+     ```
+     port:80,443,8080
+     ```
+   - Hosts in a specific network range can be found like this:
+     ```
+     net:192.168.1.1/24
+     ```

A useful reference on Shodan search options and operators can be found at:
https://pen-testing.sans.org/blog/2015/12/08/effective-shodan-searches.

# theHarvester

**theHarvester** is a command-line tool included in Kali Linux that acts as a wrapper for a variety of search engines and is used to find email accounts, subdomain names, virtual hosts, open ports / banners, and employee names related to a domain from different public sources (such as search engines and PGP key servers). In recent versions, the authors added the capability of doing DNS brute force, reverse IP resolution, and **Top-Level Domain (TLD)** expansion.