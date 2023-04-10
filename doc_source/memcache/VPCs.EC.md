# Understanding ElastiCache and Amazon VPCs<a name="VPCs.EC"></a>

ElastiCache is fully integrated with the Amazon Virtual Private Cloud \(Amazon VPC\)\. For ElastiCache users, this means the following:
+ If your AWS account supports only the EC2\-VPC platform, ElastiCache always launches your cluster in an Amazon VPC\.
+ If you're new to AWS, your clusters will be deployed into an Amazon VPC\. A default VPC will be created for you automatically\.
+ If you have a default VPC and don't specify a subnet when you launch a cluster, the cluster launches into your default Amazon VPC\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#detecting-platform)\.

With Amazon Virtual Private Cloud, you can create a virtual network in the AWS cloud that closely resembles a traditional data center\. You can configure your Amazon VPC, including selecting its IP address range, creating subnets, and configuring route tables, network gateways, and security settings\.

The basic functionality of ElastiCache is the same in a virtual private cloud; ElastiCache manages software upgrades, patching, failure detection and recovery whether your clusters are deployed inside or outside an Amazon VPC\.

ElastiCache cache nodes deployed outside an Amazon VPC are assigned an IP address to which the endpoint/DNS name resolves\. This provides connectivity from Amazon Elastic Compute Cloud \(Amazon EC2\) instances\. When you launch an ElastiCache cluster into an Amazon VPC private subnet, every cache node is assigned a private IP address within that subnet\.

## Overview of ElastiCache in an Amazon VPC<a name="ElastiCacheAndVPC.Overview"></a>

The following diagram and table describe the Amazon VPC environment, along with ElastiCache clusters and Amazon EC2 instances that are launched in the Amazon VPC\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/vpc-overview-diagram.png)


|  |  | 
| --- |--- |
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/1.png)  |  The Amazon VPC is an isolated portion of the AWS Cloud that is assigned its own block of IP addresses\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/2.png)  |  An Internet gateway connects your Amazon VPC directly to the Internet and provides access to other AWS resources such as Amazon Simple Storage Service \(Amazon S3\) that are running outside your Amazon VPC\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/3.png)  |  An Amazon VPC subnet is a segment of the IP address range of an Amazon VPC where you can isolate AWS resources according to your security and operational needs\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/4.png)  |  A routing table in the Amazon VPC directs network traffic between the subnet and the Internet\. The Amazon VPC has an implied router, which is symbolized in this diagram by the circle with the R\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/5.png)  |  An Amazon VPC security group controls inbound and outbound traffic for your ElastiCache clusters and Amazon EC2 instances\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/6.png)  |  You can launch an ElastiCache cluster in the subnet\. The cache nodes have private IP addresses from the subnet's range of addresses\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/7.png)  |  You can also launch Amazon EC2 instances in the subnet\. Each Amazon EC2 instance has a private IP address from the subnet's range of addresses\. The Amazon EC2 instance can connect to any cache node in the same subnet\.  | 
|  ![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/callouts/8.png)  |  For an Amazon EC2 instance in your Amazon VPC to be reachable from the Internet, you need to assign a static, public address called an Elastic IP address to the instance\.  | 

## Why use the Amazon VPC instead of EC2 classic with your ElastiCache deployment?<a name="VPCs.WhyUse"></a>


|  | 
| --- |
| We are retiring EC2\-Classic on August 15, 2022\. We recommend that you migrate from EC2\-Classic to a VPC\. For more information, see [Migrating an EC2\-Classic cluster into a VPC](Migrating-ec2-classic_to_VPC.md) and the blog [EC2\-Classic Networking is Retiring – Here’s How to Prepare](http://aws.amazon.com/blogs/aws/ec2-classic-is-retiring-heres-how-to-prepare/)\. | 

Launching your instances into an Amazon VPC allows you to:
+ Assign static private IP addresses to your instances that persist across starts and stops\.
+ Assign multiple IP addresses to your instances\.
+ Define network interfaces, and attach one or more network interfaces to your instances\.
+ Change security group membership for your instances while they're running\.
+ Control the outbound traffic from your instances \(egress filtering\) in addition to controlling the inbound traffic to them \(ingress filtering\)\.
+ Add an additional layer of access control to your instances in the form of network access control lists \(ACL\)\.
+ Run your instances on single\-tenant hardware\.

For a comparison of Amazon EC2 Classic, Default VPC, and Non\-default VPC, see [Differences Between EC2\-Classic and EC2\-VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html#differences-ec2-classic-vpc)\.

 The Amazon VPC must allow non\-dedicated Amazon EC2 instances\. You cannot use ElastiCache in an Amazon VPC that is configured for dedicated instance tenancy\.

## Prerequisites<a name="ElastiCacheAndVPC.Prereqs"></a>

To create an ElastiCache cluster within an Amazon VPC, your Amazon VPC must meet the following requirements:
+ The Amazon VPC must allow nondedicated Amazon EC2 instances\. You cannot use ElastiCache in an Amazon VPC that is configured for dedicated instance tenancy\.
+ A cache subnet group must be defined for your Amazon VPC\. ElastiCache uses that cache subnet group to select a subnet and IP addresses within that subnet to associate with your cache nodes\.
+ A cache security group must be defined for your Amazon VPC, or you can use the default provided\.
+ CIDR blocks for each subnet must be large enough to provide spare IP addresses for ElastiCache to use during maintenance activities\.

## Routing and security<a name="ElastiCacheAndVPC.RoutingAndSecurity"></a>

You can configure routing in your Amazon VPC to control where traffic flows \(for example, to the Internet gateway or virtual private gateway\)\. With an Internet gateway, your Amazon VPC has direct access to other AWS resources that are not running in your Amazon VPC\. If you choose to have only a virtual private gateway with a connection to your organization's local network, you can route your Internet\-bound traffic over the VPN and use local security policies and firewall to control egress\. In that case, you incur additional bandwidth charges when you access AWS resources over the Internet\.

You can use Amazon VPC security groups to help secure the ElastiCache clusters and Amazon EC2 instances in your Amazon VPC\. Security groups act like a firewall at the instance level, not the subnet level\.

**Note**  
We strongly recommend that you use DNS names to connect to your cache nodes, as the underlying IP address can change if you reboot the cache node\.

## Amazon VPC documentation<a name="ElastiCacheAndVPC.VPCDocs"></a>

Amazon VPC has its own set of documentation to describe how to create and use your Amazon VPC\. The following table gives links to the Amazon VPC guides\.


| Description | Documentation | 
| --- | --- | 
| How to get started using Amazon VPC | [Getting started with Amazon VPC](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-getting-started.html) | 
| How to use Amazon VPC through the AWS Management Console | [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/) | 
| Complete descriptions of all the Amazon VPC commands | [Amazon EC2 Command Line Reference](https://docs.aws.amazon.com/AWSEC2/latest/CommandLineReference/) \(the Amazon VPC commands are found in the Amazon EC2 reference\) | 
| Complete descriptions of the Amazon VPC API operations, data types, and errors | [Amazon EC2 API Reference](https://docs.aws.amazon.com/AWSEC2/latest/APIReference/) \(the Amazon VPC API operations are found in the Amazon EC2 reference\) | 
| Information for the network administrator who needs to configure the gateway at your end of an optional IPsec VPN connection | [What is AWS Site\-to\-Site VPN?](https://docs.aws.amazon.com/vpn/latest/s2svpn/VPC_VPN.html) | 

For more detailed information about Amazon Virtual Private Cloud, see [Amazon Virtual Private Cloud](https://aws.amazon.com/vpc/)\.