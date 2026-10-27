The management service creates user clusters. Management service uses a CLuster Operator with resources AWSClusters and AWSNodePools to create the user clusters’ controlplane and node pools respectively. The IP address per user cluster is defined in the AWSCluster resource.

Below is an example of an AWSCluster resource.

```
apiVersion: multicloud.cluster.gke.io/v1
kind: AWSCluster
metadata:
  name: CLUSTER_NAME
spec:
  region: AWS_REGION
  networking:                     #A
    vpcID: VPC_ID
    podAddressCIDRBlocks: POD_ADDRESS_CIDR_BLOCKS
    serviceAddressCIDRBlocks: SERVICE_ADDRESS_CIDR_BLOCKS
    ServiceLoadBalancerSubnetIDs: SERVICE_LOAD_BALANCER_SUBNETS
  controlPlane:                   #B
    version: CLUSTER_VERSION
    instanceType: AWS_INSTANCE_TYPE
    keyName: SSH_KEY_NAME
    subnetIDs:
    - CONTROL_PLANE_SUBNET_IDS
    securityGroupIDs:
    - CONTROL_PLANE_SECURITY_GROUPS
    iamInstanceProfile: CONTROL_PLANE_IAM_ROLE
    rootVolume:
      sizeGiB: ROOT_VOLUME_SIZE
    etcd:
      mainVolume.sizeGiB: ETCD_VOLUME_SIZE
  databaseEncryption:
    kmsKeyARN: ARN_OF_KMS_KEY
  hub: # Optional
    membershipName: ANTHOS_CONNECT_NAME
  workloadIdentity: # Optional
    oidcDiscoveryGCSBucket: WORKLOAD_IDENTITY_BUCKET
```

You define the required IP addresses in the networking section.

Anthos on AWS cluster requires the following IP addresses.

+   1. **Node IP** - Node IPs are assigned to the EC2 instances as they are created. Each EC2 instance is assigned a single IP from the private