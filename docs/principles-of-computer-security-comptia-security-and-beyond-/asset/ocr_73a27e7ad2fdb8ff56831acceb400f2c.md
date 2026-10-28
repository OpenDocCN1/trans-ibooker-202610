# FIGURE 13-1 Internet Control Message Protocol (ICMP) sniffed in Snort

d. Enter the following command:

```
sudo snort -v -i lo
```

To see the difference, open up a new terminal, ping the loopback address (127.0.0.1), and observe the output in the terminal in which Snort is sniffing.

You can break out with CTRL-C.

3a–3c

# Step 3 Run Snort, generating upper layer data, in addition to Layer 3 and Layer 4 header information.

a. Enter the following command:

```
sudo snort -vd
```

The following is from the Snort man page:

-d Dump the application layer data when displaying packets in verbose or packet logging mode.

After running Snort with the -v and -d options together (-vd), stop Snort with CTRL-C.

Snort runs and shows the upper layer data, in addition to information from Layer 3 and Layer 4 (when TCP or UDP is used), as shown in Figure 13-2.