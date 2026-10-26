![](bbox=[0.169, 0.106, 0.831, 0.587])

+   12. Close and reopen the package to force the expression to be considered.
+   
+   13. Double-click on it to open the **HDF File Source** editor and assign the following:
+      - **Hadoop Connection Manager:** Select the `cmgr_Hadoop_Sandbox` from the drop-down list.
+      - **File Path:** It should be `/SSISCookBook/Export/000000_0`. This is the value of the variable expression we set earlier.
+      - **File format:** Leave the default, `Text`.
+      - **Column delimiter character:** Use the vertical bar (`|`).