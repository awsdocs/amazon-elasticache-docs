# Choosing a network type<a name="network-type"></a>

When creating a cache cluster using Memcached engine version 1\.5\.10 onward, you choose between using Internet Protocol version 4 \(IPV4\), Internet Protocol version 6 \(IPV6\) or dual stack network type to connect with the cluster\.A dual\-stack network type allows you to connect to the cluster over IPV4 and IPV6\.

**Note**  
IPV6 is only supported on T4g node types\.

ElastiCache is fully integrated with Amazon VPC\. For ElastiCache users, this means the following:
+ If your AWS account supports only the EC2\-VPC platform, ElastiCache always launches your cluster in a VPC\.
+ If you're new to AWS, your clusters will be deployed into a VPC\. A default VPC will be created for you automatically\.
+ If you have a default VPC and don't specify a subnet when you launch a cluster, the cluster launches into your default Amazon VPC\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#detecting-platform)\.

With Amazon VPC, you can create a virtual network in the AWS Cloud that closely resembles a traditional data center\. You can configure your VPC, including selecting its IP address range, creating subnets, and configuring route tables, network gateways, and security settings\.

The basic functionality of ElastiCache is the same in a virtual private cloud\. ElastiCache manages software upgrades, patching, failure detection, and recovery whether your clusters are deployed inside or outside a VPC\.

ElastiCache cache nodes deployed outside a VPC are assigned an IP address to which the endpoint/DNS name resolves\. This provides connectivity from Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. When you launch an ElastiCache cluster into a VPC private subnet, every cache node is assigned a private IP address within that subnet\.

## Adding subnets<a name="network-type-subnets"></a>

A subnet group is a collection of subnets \(typically private\) that you can designate for your clusters running in an Amazon Virtual Private Cloud \(VPC\) environment\. If you create a cluster in an Amazon VPC, you must specify a subnet group\. The subnet group you specify will depend on which network type you chose\. If you choose IPV4 as your network type, a default subnet group will be available or you can choose to create a new one\. ElastiCache uses that subnet group to choose a subnet and IP addresses within that subnet to associate with your nodes\.

If you choose dual\-stack or IPV6, you will be directed to create dual\-stack or IPV6 subnets\. For more information, see [Create a subnet in your VPC](https://docs.aws.amazon.com/vvpc/latest/userguide/working-with-vpcs.html#AddaSubnet)