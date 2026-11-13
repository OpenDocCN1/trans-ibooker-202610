
# Testing CloudFormation Stacks

## Deploying a Broken ASG Template

```bash
systemctl start yolo
```

```yaml
SecurityGroups:
  - !Ref Sg

Asg:
  Type: AWS::AutoScaling::AutoScalingGroup
  Properties:
    # ...
    LaunchConfigurationName: !Ref Lc
    # ...
```

# The rest of the resources...

```yaml
Outputs:
  Dns:
    Value: !GetAtt Elb.DNSName
```

When we deploy this template, CloudFormation will report that the creation of the stack is complete:

```bash
aws cloudformation deploy \
  --template-file broken_asg.yaml \
  --stack-name broken \
  --parameter-overrides VpcId=vpc-12345678 \
  SubnetIds=subnet-123,subnet-456,subnet-789,subnet-012,subnet-345,subnet-678
```

```
Waiting for changeset to be created..
Waiting for stack create/update to complete
Successfully created/updated stack - broken
```

However, if we try to go via the URL of the ELB from the CloudFormation output, we will see the following:

```bash
aws cloudformation describe-stacks --stack-name broken | jq .[][][].Outputs[].OutputValue
```

```
"broken-Elb-157OHF7S5UYIC-1875871321.us-east-1.elb.amazonaws.com"
```

```bash
curl broken-Elb-157OHF7S5UYIC-1875871321.us-east-1.elb.amazonaws.com
```

```
<html>
<head><title>503 Service Temporarily Unavailable</title></head>
<body bgcolor="white">
<center><h1>503 Service Temporarily Unavailable</h1></center>
</body>
</html>
```

Why does this happen? If the stack is created, that means the infrastructure is in place and everything *should* be fine. However, if we open the EC2 console, we can see that the instances are being terminated one by one:

![Figure 4.1 – The AWS EC2 console](image/B10533_04_001.jpg)

Figure 4.1 – The AWS EC2 console

This occurs because the launch configuration's *user data* is malfunctioning (as you may have noticed, I haven't installed any web server software). An ASG has also been created and the instances are being launched and terminated, but CloudFormation is not aware of that; it received the API response from EC2 and the ASG.

There is an easy way to solve this problem using ASGs (we will look into this in [Chapter 6](#B10533_06_Final_NM_ePUB.xhtml#_idTextAnchor118), *Configuration Management of the EC2 Instances Using cfn-init*), but for now, just keep in mind that these kinds of issues might occur and CloudFormation cannot deal with them out of the box.

Another problem with testing our templates is that CloudFormation doesn't have any unit testing abilities, such as common programming languages. The only way to test them is to provision the stack from the template and run tests against the stack and its resources.

There are several ways of checking application stacks, but the simplest is to make an HTTP request to a health check endpoint (in our case, `/`).

Our test suite will be a small Python script, which will do the following:

1. Parse the argument (the stack name).
2. Make a request to the CloudFormation API to get the stack output (the ELB DNS name).
3. Make an HTTP request to that DNS name.
4. If the script doesn't receive a `200 OK` HTTP response, it will print `Stack deployment failed!` and exit with code `1`.

This is how our testing script will look:

### asg_test.py

```python
# imports ...

try:
    stack = sys.argv[1]
except IndexError:
    print("Please provide a stack name")
    sys.exit(1)

cfn_client = boto3.client('cloudformation')

try:
    stack = cfn_client.describe_stacks(StackName=stack)
except botocore.exceptions.ClientError:
    print("This stack does not exist or region is incorrect")
    sys.exit(1)

elb_dns = stack['Stacks'][0]['Outputs'][0]['OutputValue']

for _ in range(0, 2):
    resp = requests.get(f"http://{elb_dns}")
    if resp.status_code == 200:
        print("Test succeeded")
        sys.exit(0)
    sleep(5)

print(f"Result of test: {resp.content}")
print(f"HTTP Response code: {resp.status_code}")
print("Test did not succeed")
sys.exit(1)
```

If we run it against our stack again, we will get the following result:

```bash
python asg_test.py broken
```

```
Result of test: b'<html>\r\n<head><title>503 Service Temporarily Unavailable</title></head>\r\n<body bgcolor="white">\r\n<center><h1>503 Service Temporarily Unavailable</h1></center>\r\n</body>\r\n</html>\r\n'
HTTP Response code: 503
Test did not succeed
```

Now, we have to figure out what went wrong with the template. Once we find a problem (which, in this case, is in the launch configuration's *user data*), we can fix it and rerun the stack deployment and test our resources again:

### working_asg.yaml

```yaml
# Parameters section...
Resources:
  Lc:
    Type: AWS::AutoScaling::LaunchConfiguration
    Properties:
      ImageId: !Ref AmiId
      InstanceType: t2.micro
      UserData:
        Fn::Base64: |
          #!/bin/bash
          amazon-linux-extras install -y epel
          yum -y install nginx
          systemctl start nginx
      SecurityGroups:
        - !Ref Sg

# the rest of the template...
```

Once the working template is deployed, we can run our script again:

```bash
python asg_test.py broken
```

```
Test succeeded
```

While this is just an example of this case, we know that we cannot use this test suite for other stacks and resources. Depending on what we are building, we must create a new test case for every application or infrastructure that we deploy.

For services, such as VPC networks, `IAM` roles, and DynamoDB tables, CloudFormation can ensure that resources are properly configured. But how do we know that they are doing what they are supposed to do?

## Smoke Testing VPC Resources

Let's look again at our *core* template. We know it has *private* and *public* subnets. Let's say we want to check whether subnets that are supposed to be private are associated with a private route table (the one that routes traffic through a NAT gateway).

To carry out this test, we need to do the following:

1. First, append your subnet resources with an extra tag.

   Use the following tag for public subnets:
   ```yaml
   - Key: 'Private'
     Value: 'False'
   ```

   Use the following tag for private subnets:
   ```yaml
   - Key: 'Private'
     Value: 'True'
   ```

2. Our script will gather the stack's resources, not its outputs. Filter the resources to be of the `AWS::EC2::Subnet` type and make the value of the `Private` tag `True`.

3. Obtain the ID of the private route table and then check whether private subnets are associated with the private Route Table ID.

4. The following steps will give us a test suite for the VPC subnets:

5. First, parse the arguments and verify that the stack exists:

   ### core_subnets.py

   ```python
   # imports...

   matches = []

   try:
       stack = sys.argv[1]
   except IndexError:
       print("Please provide a stack name")
       sys.exit(1)

   cfn_client = boto3.client('cloudformation')

   try:
       resources = cfn_client.describe_stack_resources(StackName=stack)
   except botocore.exceptions.ClientError:
       print("This stack does not exist or region is incorrect")
       sys.exit(1)
   ```

6. Then, obtain the ID of the private route table and all of the subnets in the stack:

   ### core_subnets.py (continued)

   ```python
   subnets_in_stack = []

   for resource in resources['StackResources']:
       if resource['LogicalResourceId'] == "PrivateRouteTable":
           private_route_table = resource['PhysicalResourceId']
       if resource['ResourceType'] == "AWS::EC2::Subnet":
           subnets_in_stack.append(resource['PhysicalResourceId'])
   ```

7. Finally, obtain the subnets' association using the EC2 API, match their tags, and check that they have the necessary route table association:

   ### core_subnets.py (continued)

   ```python
   ec2_client = boto3.client('ec2')

   subnets_to_check = []

   for subnet in subnets_in_stack:
       resp = ec2_client.describe_subnets(SubnetIds=[subnet])
       for tag in resp['Subnets'][0]['Tags']:
           if tag['Key'] == "Private" and tag['Value'] == "True":
   ```
