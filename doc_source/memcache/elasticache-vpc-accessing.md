# Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC<a name="elasticache-vpc-accessing"></a>

Amazon ElastiCache supports the following scenarios for accessing a cluster in an Amazon VPC:

**Contents**
+ [Accessing an ElastiCache Cluster when it and the Amazon EC2 Instance are in the Same Amazon VPC](#elasticache-vpc-accessing-same-vpc)
+ [Accessing an ElastiCache Cluster when it and the Amazon EC2 Instance are in Different Amazon VPCs](#elasticache-vpc-accessing-different-vpc)
  + [In Different Amazon VPCs in the Same Region](#elasticache-vpc-accessing-different-vpc-same-region)
    + [Using Transit Gateway](#elasticache-vpc-accessing-using-transit-gateway)
  + [In Different Amazon VPCs in Different Regions](#elasticache-vpc-accessing-different-vpc-different-region)
    + [Using Transit VPC](#elasticache-vpc-accessing-different-vpc-different-region-using-transit-vpc)
+ [Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center](#elasticache-vpc-accessing-data-center)
  + [Using VPN Connectivity](#elasticache-vpc-accessing-data-center-vpn)
  + [Using Direct Connect](#elasticache-vpc-accessing-data-center-direct-connect)

## Accessing an ElastiCache Cluster when it and the Amazon EC2 Instance are in the Same Amazon VPC<a name="elasticache-vpc-accessing-same-vpc"></a>

The most common use case is when an application deployed on an EC2 instance needs to connect to a Cluster in the same VPC\.

The following diagram illustrates this scenario

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-inVPC-AccessedByEC2-SameVPC.png)

The simplest way to manage access between EC2 instances and clusters in the same VPC is to do the following:

1. Create a VPC security group for your cluster\. This security group can be used to restrict access to the cluster instances\. For example, you can create a custom rule for this security group that allows TCP access using the port you assigned to the cluster when you created it and an IP address you will use to access the cluster\. 

   The default port for Memcached clusters is `11211`\.

1. Create a VPC security group for your EC2 instances \(web and application servers\)\. This security group can, if needed, allow access to the EC2 instance from the Internet via the VPC's routing table\. For example, you can set rules on this security group to allow TCP access to the EC2 instance over port 22\.

1. Create custom rules in the security group for your Cluster that allow connections from the security group you created for your EC2 instances\. This would allow any member of the security group to access the clusters\.

**Note**  
If you are planning to use [Local Zones](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Local_zones.html), ensure that you have enabled them\. When you create a subnet group in that local zone, your VPC is extended to that Local Zone and your VPC will treat the subnet as any subnet in any other Availability Zone\. All relevant gateways and route tables will be automatically adjusted\.

**To create a rule in a VPC security group that allows connections from another security group**

1. Sign in to the AWS Management Console and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc](https://console.aws.amazon.com/vpc)\.

1. In the navigation pane, choose **Security Groups**\.

1. Select or create a security group that you will use for your Cluster instances\. Under **Inbound Rules**, select **Edit Inbound Rules** and then select **Add Rule**\. This security group will allow access to members of another security group\.

1. From **Type** choose **Custom TCP Rule**\.

   1. For **Port Range**, specify the port you used when you created your cluster\.

      The default port for Memcached clusters is `11211`\.

   1. In the **Source** box, start typing the ID of the security group\. From the list select the security group you will use for your Amazon EC2 instances\.

1. Choose **Save** when you finish\.  
![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/VPC-Rules.png)

## Accessing an ElastiCache Cluster when it and the Amazon EC2 Instance are in Different Amazon VPCs<a name="elasticache-vpc-accessing-different-vpc"></a>

When your Cluster is in a different VPC from the EC2 instance you are using to access it, there are several ways to access the cluster\. If the Cluster and EC2 instance are in different VPCs but in the same region, you can use VPC peering\. If the Cluster and the EC2 instance are in different regions, you can create VPN connectivity between regions\.

**Topics**
+ [In Different Amazon VPCs in the Same Region](#elasticache-vpc-accessing-different-vpc-same-region)
+ [In Different Amazon VPCs in Different Regions](#elasticache-vpc-accessing-different-vpc-different-region)

 

### Accessing an ElastiCache Cluster when it and the Amazon EC2 Instance are in Different Amazon VPCs in the Same Region<a name="elasticache-vpc-accessing-different-vpc-same-region"></a>

The following diagram illustrates accessing a cluster by an Amazon EC2 instance in a different Amazon VPC in the same region using an Amazon VPC peering connection\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-inVPC-AccessedByEC2-DifferentVPC.png)

*Cluster accessed by an Amazon EC2 instance in a different Amazon VPC within the same Region \- VPC Peering Connection*

A VPC peering connection is a networking connection between two VPCs that enables you to route traffic between them using private IP addresses\. Instances in either VPC can communicate with each other as if they are within the same network\. You can create a VPC peering connection between your own Amazon VPCs, or with an Amazon VPC in another AWS account within a single region\. To learn more about Amazon VPC peering, see the [VPC documentation](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-peering.html)\.

**To access a cluster in a different Amazon VPC over peering**

1. Make sure that the two VPCs do not have an overlapping IP range or you will not be able to peer them\.

1. Peer the two VPCs\. For more information, see [Creating and Accepting an Amazon VPC Peering Connection](https://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/create-vpc-peering-connection.html)\.

1. Update your routing table\. For more information, see [Updating Your Route Tables for a VPC Peering Connection](https://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/vpc-peering-routing.html)

   Following is what the route tables look like for the example in the preceeding diagram\. Note that **pcx\-a894f1c1** is the peering connection\.  
![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/VPC-RoutingTable.png)

   *VPC Routing Table*

1. Modify the Security Group of your ElastiCache cluster to allow inbound connection from the Application security group in the peered VPC\. For more information, see [Reference Peer VPC Security Groups](https://docs.aws.amazon.com/AmazonVPC/latest/PeeringGuide/vpc-peering-security-groups.html)\.

Accessing a cluster over a peering connection will incur additional data transfer costs\.

#### Using Transit Gateway<a name="elasticache-vpc-accessing-using-transit-gateway"></a>

A transit gateway enables you to attach VPCs and VPN connections in the same AWS Region and route traffic between them\. A transit gateway works across AWS accounts, and you can use AWS Resource Access Manager to share your transit gateway with other accounts\. After you share a transit gateway with another AWS account, the account owner can attach their VPCs to your transit gateway\. A user from either account can delete the attachment at any time\.

You can enable multicast on a transit gateway, and then create a transit gateway multicast domain that allows multicast traffic to be sent from your multicast source to multicast group members over VPC attachments that you associate with the domain\.

You can also create a peering connection attachment between transit gateways in different AWS Regions\. This enables you to route traffic between the transit gateways' attachments across different Regions\.

For more information, see [Transit gateways](https://docs.aws.amazon.com/vpc/latest/tgw/tgw-transit-gateways.html)\.

### Accessing an ElastiCache Cluster when it and the Amazon EC2 Instance are in Different Amazon VPCs in Different Regions<a name="elasticache-vpc-accessing-different-vpc-different-region"></a>

#### Using Transit VPC<a name="elasticache-vpc-accessing-different-vpc-different-region-using-transit-vpc"></a>

An alternative to using VPC peering, another common strategy for connecting multiple, geographically disperse VPCs and remote networks is to create a transit VPC that serves as a global network transit center\. A transit VPC simplifies network management and minimizes the number of connections required to connect multiple VPCs and remote networks\. This design can save time and effort and also reduce costs, as it is implemented virtually without the traditional expense of establishing a physical presence in a colocation transit hub or deploying physical network gear\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-inVPC-AccessedByEC2-DifferentVPC-DifferentRegion-VPN.png)

*Connecting across different VPCs in different regions*

Once the Transit Amazon VPC is established, an application deployed in a “spoke” VPC in one region can connect to an ElastiCache cluster in a “spoke” VPC within another region\. 

**To access a cluster in a different VPC within a different AWS Region**

1. Deploy a Transit VPC Solution\. For more information, see, [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/)\.

1. Update the VPC routing tables in the App and Cache VPCs to route traffic through the VGW \(Virtual Private Gateway\) and the VPN Appliance\. In case of Dynamic Routing with Border Gateway Protocol \(BGP\) your routes may be automatically propagated\.

1. Modify the Security Group of your ElastiCache cluster to allow inbound connection from the Application instances IP range\. Note that you will not be able to reference the application server Security Group in this scenario\.

Accessing a cluster across regions will introduce networking latencies and additional cross\-region data transfer costs\.

## Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center<a name="elasticache-vpc-accessing-data-center"></a>

Another possible scenario is a Hybrid architecture where clients or applications in the customer’s data center may need to access an ElastiCache Cluster in the VPC\. This scenario is also supported providing there is connectivity between the customers’ VPC and the data center either through VPN or Direct Connect\.

**Topics**
+ [Using VPN Connectivity](#elasticache-vpc-accessing-data-center-vpn)
+ [Using Direct Connect](#elasticache-vpc-accessing-data-center-direct-connect)

 

### Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center Using VPN Connectivity<a name="elasticache-vpc-accessing-data-center-vpn"></a>

The following diagram illustrates accessing an ElastiCache cluster from an application running in your corporate network using VPN connections\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-inVPC-AccessedByAppInCustDataCenter-VPN.png)

*Connecting to ElastiCache from your data center via a VPN*

**To access a cluster in a VPC from on\-prem application over VPN connection**

1. Establish VPN Connectivity by adding a hardware Virtual Private Gateway to your VPC\. For more information, see [Adding a Hardware Virtual Private Gateway to Your VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html)\.

1. Update the VPC routing table for the subnet where your ElastiCache cluster is deployed to allow traffic from your on\-premises application server\. In case of Dynamic Routing with BGP your routes may be automatically propagated\.

1. Modify the Security Group of your ElastiCache cluster to allow inbound connection from the on\-premises application servers\.

Accessing a cluster over a VPN connection will introduce networking latencies and additional data transfer costs\.

 

### Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center Using Direct Connect<a name="elasticache-vpc-accessing-data-center-direct-connect"></a>

The following diagram illustrates accessing an ElastiCache cluster from an application running on your corporate network using Direct Connect\.

![\[\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-inVPC-AccessedByAppInCustDataCenter-DirectConnect.png)

*Connecting to ElastiCache from your data center via Direct Connect*

**To access an ElastiCache cluster from an application running in your network using Direct Connect**

1. Establish Direct Connect connectivity\. For more information, see, [Getting Started with AWS Direct Connect](http://docs.aws.amazon.com/directconnect/latest/UserGuide/getting_started.html)\.

1. Modify the Security Group of your ElastiCache cluster to allow inbound connection from the on\-premises application servers\.

Accessing a cluster over DX connection may introduce networking latencies and additional data transfer charges\.