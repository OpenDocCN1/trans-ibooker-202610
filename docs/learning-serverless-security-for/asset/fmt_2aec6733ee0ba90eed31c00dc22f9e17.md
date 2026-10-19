
1. Replace `[S3 BUCKET NAME]` with the actual name of your S3 bucket and then open the following URL in your browser:

```
http://[S3 BUCKET NAME].s3.amazonaws.com/
```

###### Note

If your bucket name is `serverless-static-website-abc123`, the URL will be *http​://serverless-static-website-abc123.s3​.ama⁠zonaws.com/*.

This returns an XML response that looks similar to the following:

```
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
 <Name>serverless-static-website-...</Name>
 <Prefix/>
 <Marker/>
 <MaxKeys>1000</MaxKeys>
 <IsTruncated>false</IsTruncated>
 <Contents>
  <Key>backup/lambda_function.py</Key>
  <LastModified>...</LastModified>
  ...
 </Contents>
 <Contents>
  <Key>index.html</Key>
  <LastModified>...</LastModified>
  ...
 </Contents>
</ListBucketResult>
```

As shown in the XML response, the S3 bucket contains the key `backup/lambda_function.py`, which is potentially accessible if the bucket permissions allow public read access.

2. Replace `[S3 BUCKET NAME]` with the actual name of your S3 bucket and then open the following URL in your browser to attempt direct access to the file:

```
http://[S3 BUCKET NAME].s3.amazonaws.com/backup/lambda_function.py
```

If the file is publicly accessible, this displays the `lambda_function.py` code directly in your browser:

```
from os import environ
get_env = environ.get

HARDCODED_KEY_01 = "ABCDE"
HARDCODED_KEY_02 = "FGHIJ"

...
```

###### Note

If the source code contains hardcoded keys, attackers who can access this file will be able to retrieve and misuse those credentials. They may also learn how certain components of your application are implemented, which can reveal implementation and configuration details that help them identify and exploit additional vulnerabilities.

3. Open a terminal on your local machine and then run the following command to remove any stored AWS credentials:

```
rm ~/.aws/credentials
```

###### Tip

Alternatively, you can create a backup of the credentials file before deleting it—for example, by running `mv ~/.aws/​cre⁠dentials ~/.aws/credentials.bak`. You can then use `aws configure list` to verify that no profile is currently configured and that no credentials are in use by the AWS CLI.

4. Replace `[S3 BUCKET NAME]` with the actual name of your S3 bucket and then run the following command to store the bucket name in a shell variable:

```
BUCKET_NAME="[S3 BUCKET NAME]"
```

5. List the contents of the bucket to check whether public listing is allowed:

```
aws s3 ls s3://$BUCKET_NAME/
```

This outputs the following message:

```
Unable to locate credentials. You can configure credentials by running "aws configure".
```

6. Run the following command to attempt an unauthenticated list of the bucket contents:

```
aws s3 ls s3://$BUCKET_NAME/ --no-sign-request
```

This yields the following output:

```
                               PRE backup/
    2025-00-00 00:00:00       3257 index.html
```

7. Let’s repeat the preceding step, but this time list all objects recursively to view the files inside nested folders:

```
aws s3 ls s3://$BUCKET_NAME/ --recursive --no-sign-request
```

Here is the output:

```
2025-00-00 00:00:00        875 backup/lambda_function.py
2025-00-00 00:00:00       3257 index.html
```

8. To retrieve a more detailed output, use the `aws s3api list-objects` command to output the bucket contents in JSON format:

```
aws s3api list-objects --bucket $BUCKET_NAME --no-sign-request
```

You should see a JSON response resembling the following output:

```
{
    "Contents": [
        {
            "Key": "backup/lambda_function.py",
            "LastModified": "2025-00-00T00:00:00+00:00",
            "ETag": "\"...\"",
            "ChecksumAlgorithm": [
                "CRC64NVME"
            ],
            "ChecksumType": "FULL_OBJECT",
            "Size": 875,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "...",
                "ID": "..."
            }
        },
        {
            "Key": "index.html",
            "LastModified": "2025-00-00T00:00:00+00:00",
            "ETag": "\"...\"",
            "ChecksumAlgorithm": [
                "CRC64NVME"
            ],
            "ChecksumType": "FULL_OBJECT",
            "Size": 3257,
            "StorageClass": "STANDARD",
            "Owner": {
                "DisplayName": "...",
                "ID": "..."
            }
        }
    ],
    "RequestCharged": null,
    "Prefix": ""
}
```

9. Create a directory for the downloaded files and then check whether you can download a specific file from the bucket without authentication:

```
mkdir downloaded_files

aws s3 cp s3://$BUCKET_NAME/backup/lambda_function.py \
    ./downloaded_files/ --no-sign-request
```

This should successfully download the `lambda_function.py` file into the `downloaded_files` directory:

```
download: s3://serverless-static-website.../backup/lambda_function.py
to downloaded_files/lambda_function.py
```

10. Create a sample file and attempt to upload it to the S3 bucket without authentication:

```
touch sample.txt

aws s3 cp ./sample.txt s3://$BUCKET_NAME/ --no-sign-request
```

Since the bucket does not allow public write access, this command should fail with an `AccessDenied` error:

```
upload failed: ./sample.txt to
s3://serverless-static-website.../sample.txt An error occurred
(AccessDenied) when calling the PutObject operation: Access Denied
```

In practice, teams may reuse the same S3 bucket for multiple purposes, such as hosting static assets, storing backups, and sometimes even storing application code and configuration, which can unintentionally expose sensitive files and configuration data alongside the hosted website. Attackers don’t assume that an S3 bucket contains only HTML or JavaScript files. They often enumerate buckets to check for any other objects, such as backups, credentials, or logfiles that could inadvertently reveal information useful for launching further attacks.

From a defensive security standpoint, you should carefully plan how each S3 bucket is used and lean toward using separate buckets for different types of data to avoid accidentally exposing sensitive information. You should ensure that each bucket has the correct access control configuration applied and enable logging to continuously monitor and audit access. In addition to these measures, you should consider implementing automated alerts to notify you or your team whenever an S3 bucket is misconfigured or becomes publicly accessible.

# Using Code Injection Attacks to Execute Arbitrary Code in Lambda Functions

In this section, you’ll explore how attackers can exploit poorly written Lambda functions by abusing dynamic code execution vulnerabilities. You’ll start by experimenting with valid input expressions to establish how the application normally behaves. You will then simulate an attacker using invalid and malicious inputs to observe how the function reacts to unexpected cases, including retrieving source code files and extracting environment variables. After retrieving AWS temporary credentials from the Lambda execution environment by using a code injection payload, you will use them to configure a new AWS CLI profile in the Attacker account.
