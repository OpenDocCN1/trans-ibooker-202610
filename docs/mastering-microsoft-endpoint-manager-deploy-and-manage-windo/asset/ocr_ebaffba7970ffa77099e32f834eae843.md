**Resolution:** Ensure the firewall access is correctly configured to allow DSC endpoints across the following:

+   - `prod.warmpath.msftcloudes.com`
+   - `cpcsacnrysalprodprna01.blob.core.windows.net` (North America)
+   - `cpcsacnrysalprodprna02.blob.core.windows.net` (North America)
+   - `cpcsacnrysalprodprap01.blob.core.windows.net` (Asia Pacific)
+   - `cpcsacnrysalprodprau01.blob.core.windows.net` (Australia)
+   - `cpcsacnrysalprodpreu01.blob.core.windows.net` (EU)
+   - `cpcsacnrysalprodpreu02.blob.core.windows.net` (EU)
+   - `mrsgglobalsteus2prod.blob.core.windows.net`
+   - `catalogartifact.azureedge.net`
+   - `kms.core.windows.net`

To check if this is a network connection URL access issue, you can try the following actions:

+   1. Perform a nslookup operation on the Azure container for your region to check you can resolve with your organization's DNS servers, as illustrated in the following screenshot:

```
C:\WINDOWS\system32>nslookup cpcsacnrysalprodpreu01.blob.core.windows.net
Server:  UnKnown
Address:  172.16.0.1

Non-authoritative answer:
Name:    blob.dub07prdstr05a.store.core.windows.net
Address:  52.239.138.100
Aliases:  cpcsacnrysalprodpreu01.blob.core.windows.net
```

Figure 17.12 – nslookup on Azure container