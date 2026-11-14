
# Providing Direct Internet Access to an EC2 Instance

## Problem

You have an existing EC2 instance in a subnet of a VPC. You need to provide the ability for this instance to directly reach clients on the internet.

## Solution

You will create an internet gateway and attach it to your VPC. Next you will modify the route table associated with the subnet where the EC2 instance lives. You will add a route that sends traffic from the subnets to the internet gateway. Finally, create an Elastic IP (EIP) and associate it with the instance, as shown in [Figure 2-4](#ch02.html#public_subnet_tiercomma_internet_gatewa).

![](assets/awsc_0204.png)

### Prerequisites

- VPC and subnets created in two AZs and associated route tables.
- EC2 instance deployed. You will need the ability to connect to this for testing.

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Networking).

### Steps

1. Create an internet gateway (IGW):

```
INET_GATEWAY_ID=$(aws ec2 create-internet-gateway \
    --tag-specifications \
'ResourceType=internet-gateway,Tags=[{Key=Name,Value=AWSCookbook202}]' \
    --output text --query InternetGateway.InternetGatewayId)
```

2. Attach the internet gateway to the existing VPC:

```
aws ec2 attach-internet-gateway \
    --internet-gateway-id $INET_GATEWAY_ID --vpc-id $VPC_ID
```

3. In each route table of your VPC, create a route that sets the default route destination to the internet gateway:

```
aws ec2 create-route --route-table-id $ROUTE_TABLE_ID_1 \
    --destination-cidr-block 0.0.0.0/0 --gateway-id $INET_GATEWAY_ID

aws ec2 create-route --route-table-id $ROUTE_TABLE_ID_2 \
    --destination-cidr-block 0.0.0.0/0 --gateway-id $INET_GATEWAY_ID
```

For each command in step 3, you should see output similar to the following:

```
{
  "Return": true
}
```

4. Create an EIP:

```
ALLOCATION_ID=$(aws ec2 allocate-address --domain vpc \
    --output text --query AllocationId)
```

#### Note

AWS defines an [Elastic IP address (EIP)](https://oreil.ly/FCrvT) as “a static IPv4 address designed for dynamic cloud computing. An EIP address is allocated to your AWS account and is yours until you release it.”

5. Associate the EIP with the existing EC2 instance:

```
aws ec2 associate-address \
    --instance-id $INSTANCE_ID --allocation-id $ALLOCATION_ID
```

You should see output similar to the following:

```
{
  "AssociationId": "eipassoc-<<snip>>"
}
```

#### Validation Checks

Connect to the EC2 instance using SSM Session Manager (see [Recipe 1.6](#ch01.html#connecting_to_ectwo_instances_using_aws)):

```
aws ssm start-session --target $INSTANCE_ID
```

Ping a host on the internet to test internet connectivity:

```
ping -c 4 homestarrunner.com
```

You should see output similar to the following:

```
sh-4.2$ ping -c 4 homestarrunner.com
PING homestarrunner.com (72.10.33.178) 56(84) bytes of data.
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=1 ttl=49 time=2.12 ms
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=2 ttl=49 time=2.04 ms
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=3 ttl=49 time=2.05 ms
64 bytes from homestarrunner.com (72.10.33.178): icmp_seq=4 ttl=49 time=2.08 ms
--- homestarrunner.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3002ms
rtt min/avg/max/mdev = 2.045/2.078/2.127/0.045 ms
sh-4.2$
```

#### Tip

The public IP is not part of the OS configuration. If you want to retrieve the public IP from the instance’s [metadata](https://oreil.ly/lmZ1U), you can use this command:

```
curl http://169.254.169.254/latest/meta-data/public-ipv4
```

Exit the Session Manager session:

```
exit
```

### Clean up

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Networking).

### Discussion

The route that you created in your route table entry sends all nonlocal traffic to the IGW that provides your VPC internet connectivity. Because you were working with an existing running EC2 instance, you needed to create an Elastic IP and associated it with the instance. These steps enabled internet communication to the instance without having to interact with it. There is an [option to enable auto-assignment](https://oreil.ly/disUG) of public IPv4 addresses for newly launched instances in a subnet. However, if you utilize auto-assignment, the public IPs will change after each instance reboot. EIPs associated with an instance will not change after reboots.

#### Note

Route tables give priority to the most specific route. AWS also allows you to create routes that are more specific than the default local route. This allows you to create very controlled network flows. More information about route priority can be found in an AWS [discussion](https://oreil.ly/DKpNO).

The security group associated with your instance does not allow inbound access. If you would like to allow inbound internet access to an instance in a public subnet, you will have to configure a security group ingress rule for this.

A subnet that has a route of `0.0.0.0/0` associated with an IGW is considered a public subnet. It is considered a security best practice to place instances only in this type of tier that require inbound access from the public internet. End-user-facing load balancers are commonly placed in public subnets. A public subnet would not be an ideal choice for an application server or a database. In these cases, you can create a private tier or an isolated tier to fit your needs with the appropriate routing and use a NAT gateway to direct that subnet traffic to the internet gateway only when outbound internet access is required.

### Challenge

Install a web server on the EC2 instance, modify the security group, and connect to the instance from your workstation. See [Recipe 2.7](#ch02.html#redirecting_http_traffic_to_https_with) for an example of how to configure internet access for instances in private subnets using a load balancer.

# 2.4 Using a NAT Gateway for Outbound Internet Access from Private Subnets

## Problem

You have public subnets in your VPC that have a route to an internet gateway. You want to leverage this setup to provide outbound-only internet access for an instance in private subnets.

## Solution

Create a NAT gateway in one of the public subnets. Then create an Elastic IP and associate it with the NAT gateway. In the route table associated with the private subnets, add a route for internet-bound traffic that targets the NAT gateway (see [Figure 2-5](#ch02.html#internet_access_for_private_subnets_pro)).

### Prerequisites

- VPC with public subnets in two AZs and associated route tables.
- Isolated subnets created in two AZs (we will turn these into the private subnets) and associated route tables.
- Two EC2 instances deployed in the isolated subnets. You will need the ability to connect to these for testing.

### Preparation

Follow the steps in this recipe’s folder in the [chapter code repository](https://github.com/AWSCookbook/Networking).

![](assets/awsc_0205.png)

### Steps

1. Create an Elastic IP to be used with the NAT gateway:

```
ALLOCATION_ID=$(aws ec2 allocate-address --domain vpc \
    --output text --query AllocationId)
```

2. Create a NAT gateway within the public subnet of AZ1:

```
NAT_GATEWAY_ID=$(aws ec2 create-nat-gateway \
    --subnet-id $VPC_PUBLIC_SUBNET_1 \
    --allocation-id $ALLOCATION_ID \
    --output text --query NatGateway.NatGatewayId)
```

3. This will take a few moments for the state to become available; check the status:

```
aws ec2 describe-nat-gateways \
    --nat-gateway-ids $NAT_GATEWAY_ID \
    --output text --query NatGateways[0].State
```
