![](bbox=[0.231, 0.109, 0.886, 0.393])

# **FIGURE 14.23** Enable Portscan

+   20. At the bottom of Step # 5, locate the following:
+   
+   ```text
+   whitelist $WHITE_LIST_PATH/white_list.rules,  \\
+   blacklist $BLACK_LIST_PATH/black_list.rules
+   ```
+   
+   Insert a pound sign (#) prior to each line, as illustrated in Figure 14.24. The pound sign disables the path associated with a whitelist and blacklist.

+   21. Within Step # 6, locate the following:
+   
+   ```text
+   # pcap
+   # output log_tcpdump: tcpdump.log
+   ```
+   
+   Insert `output alert_fast: alerts.ids` underneath, as illustrated in Figure 14.25. Inserting this line will create a log when alerted during the intrusion-detection mode.

+   22. You will need to create this file. Without closing the `snort.conf` file, click the Start icon and type Word into the search bar. Microsoft Word or WordPad will appear. Press the Enter key.

+   23. Select File > Save As. You will need to navigate to `C: > Snort > Log`. Enter `alert.ids` as the name and select Save. Select Yes if a warning prompt appears.