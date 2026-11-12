![](bbox=[0.12, 0.08, 0.88, 0.44])

# Figure 14-5 IPv4 header

In IPv6, six of these header fields are no longer part of the new IPv6 header. These are indicated by the lighter shading in Figure 14-5:

+   - **Header Length** This is no longer necessary in IPv6 because all IPv6 headers are fixed-length.
+   - **Identification** This function hasn’t been used for a long time in IPv4 and has been deprecated in IPv6.
+   - **Flags** This function hasn’t been used for a long time in IPv4 and has been deprecated in IPv6.
+   - **Fragment Offset** Fragmentation is processed differently in IPv6 and doesn’t need fields in the IPv6 header for this process (this has been moved to the Extension Header Information field in IPv6).
+   - **Header Checksum** Today’s data link layer technologies are reliable, so this field is no longer necessary in IPv6.
+   - **Padding** This field is no longer necessary in IPv6.

Figure 14-6 displays the fields of the IPv6 header. Here is an explanation of the eight fields in the IPv6 header: