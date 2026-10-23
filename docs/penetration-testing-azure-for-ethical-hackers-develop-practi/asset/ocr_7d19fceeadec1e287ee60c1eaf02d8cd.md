This will download the file into a temporary directory on the pentest Linux VM. Make a note of the download location that is highlighted in the following screenshot:

![](bbox=[0.18, 0.16, 0.815, 0.325])

Figure 6.29 – Cloud Shell files downloaded with Lava

9. After the download has been completed, exit Lava using the exit command:

```
exit
```

10. Back in the Bash shell, mount the downloaded image file using the following command. Replace <download_location> with the download path that you made a note of in step 8 of this exercise:

```
mount <download_location>/.cloudconsole/acc_azureadmin.img /mnt
```

Here is a screenshot of this:

![](bbox=[0.13, 0.545, 0.87, 0.605])

Figure 6.30 – Mounting the acc_azureadmin.img file in the Linux VM

11. Explore the mounted drive by typing the following command and pressing Tab:

```
ls /mnt/
```

You should see an output of persistent files and data from the Cloud Shell user's previous sessions. This includes sensitive information, such as the user's shell context information.