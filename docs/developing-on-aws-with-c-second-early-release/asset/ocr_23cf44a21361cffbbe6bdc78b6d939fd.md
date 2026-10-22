as shown in the following sequence, echo content into a file, and write it to the bucket created earlier. Finally, run Get-S3Object to verify the file’s creation in S3, which you can see in Figure 1-12.

```
Import-Module AWSPowerShell.NetCore
echo "This is data" > data.txt
Write-S3Object -BucketName silly-name-1234 -File data.txt
Get-S3Object
```

```
PS /home/cloudshell-user> Import-Module AWSPowerShell.NetCore
PS /home/cloudshell-user> echo "This is data" > data.txt
PS /home/cloudshell-user> Write-S3Object -BucketName silly-name-1234 -File data.txt
PS /home/cloudshell-user> Get-S3Object
```

cmdlet Get-S3Object at command pipeline position 1
Supply values for the following parameters:
BucketName: silly-name-1234

ChecksumAlgorithm : {}
ETag               : "bd7090333ff9eabec6a4c1f9461aaaff"
BucketName         : silly-name-1234
Key                : data.txt
LastModified       : 4/1/2022 6:51:54 PM
Owner              : Amazon.S3.Model.Owner
Size               : 13
StorageClass       : STANDARD

Figure 1-12. AWS CloudShell Create File S3

Another feature of the CloudShell and PowerShell environment is invoking C# code. To get started using PowerShell with C#, you can create a C#