Once the command executes, we should now see the images in the Docker images list. To verify this, issue the `docker images` command. You should see the following output:

| REPOSITORY | TAG | IMAGE ID | SIZE |
|---|---|---|---|
| ostock/organization-service | chapter12 | b1c7b262926e | 485MB |
| ostock/gatewayserver | chapter12 | 61c6fc020dcf | 450MB |
| ostock/configserver | chapter12 | 877c9d855d91 | 432MB |
| ostock/licensing-service | chapter12 | 6a76bee3e40c | 490MB |
| ostock/authentication-service | chapter12 | 5e5e74f29c2 | 452MB |
| ostock/eurekaserver | chapter12 | e6bc59ae1d87 | 451MB |

The next step is to authenticate our Docker client with our ECR registry. To achieve this, we need to obtain a password and our AWS account ID by executing the following commands:

```
aws ecr get-login-password
aws sts get-caller-identity --output text --query "Account"
```

The first command retrieves the password, and the second returns our AWS account. We’ll use both values to authenticate our Docker client. Now that we have our credentials, let’s authenticate by executing the following command:

```
docker login -u AWS -p [password]
https://[aws_account_id].dkr.ecr.[region].amazonaws.com
```

Once authenticated, the next step is to create the repositories where we will store our images. To create these repositories, let’s execute the following commands:

```
aws ecr create-repository --repository-name ostock/configserver
aws ecr create-repository --repository-name ostock/gatewayserver
aws ecr create-repository --repository-name ostock/eurekaserver
aws ecr create-repository --repository-name ostock/authentication-service
aws ecr create-repository --repository-name ostock/licensing-service
aws ecr create-repository --repository-name ostock/organization-service
```

When each command executes you should see output similar to the following:

```
{
    "repository": {
        "repositoryArn": "arn:aws:ecr:us-east-2:
            8193XXXXXXX43:repository/ostock/configserver",
        "registryId": "8193XXXXXXX43",
        "repositoryName": "ostock/configserver",
        "repositoryUri": "8193XXXXXXX43.dkr.ecr.
            us-east-2.amazonaws.com/ostock/configserver",
        "createdAt": "2020-06-18T11:53:06-06:00",
        "imageTagMutability": "MUTABLE",
        "imageScanningConfiguration": {
            "scanOnPush": false
        }
    }
}
```