17. Obtain the access key and file share and upload the image back into it using the following commands. Replace <download_location> with the location that you made a note of in Step 8 of this exercise:

```
key=$(az storage account keys list -n $storagename --query [0].value -o tsv)
```

```
csfileshare=$(az storage share list --account-key $key --account-name $storagename --query [].name -o tsv)
```

```
az storage file upload --account-key $key --account-name $storagename --share-name $csfileshare --path ".cloudconsole/acc_azureadmin.img" --source "<download_location>/.cloudconsole/acc_azureadmin.img"
```

The upload process will begin after this. This will overwrite the existing file in the share. Here is a screenshot of this:

![](bbox=[0.115, 0.38, 0.85, 0.565])

Figure 6.35 – Uploading the modified .img file