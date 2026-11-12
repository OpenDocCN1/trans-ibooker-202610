# # Review Questions

You can find the answers in the appendix.

+   **1.** Which of the following utilities allows you to scan a system's network ports and see the services offered for each port, and you don't have to be logged onto the system you are scanning?
+   
+   **A.** fuser
+   
+   **B.** lsof
+   
+   **C.** nmap
+   
+   **D.** netstat
+   
+   **E.** ss

+   **2.** Nickie needs to scan his system to see what ports are listening for incoming TCP connections. He decides to use the netstat utility. What options should he employ? (Choose all that apply.)
+   
+   **A.** -l
+   
+   **B.** -u
+   
+   **C.** -s
+   
+   **D.** -T
+   
+   **E.** -t

+   **3.** Nickie knows that the netstat utility is deprecated, so he has decided to switch to the ss utility. He needs to scan his system to see what ports are listening for incoming TCP connections. What options should he employ? (Choose all that apply.)
+   
+   **A.** -l
+   
+   **B.** -u
+   
+   **C.** -s
+   
+   **D.** -T
+   
+   **E.** -t

+   **4.** Case is trying to lock down an older Linux system. He was surprised to find that it may be offering FTP services. He discovers in the /etc/services file that these older FTP services typically run on ports 20 and 21. What lsof command should he run to see if there are active connections to the FTP services?
+   
+   **A.** lsof -i UDP
+   
+   **B.** lsof -i 20:TCP
+   
+   **C.** lsof -i ftp:TCP
+   
+   **D.** lsof -i :ftp
+   
+   **E.** lsof -i 20:ftp