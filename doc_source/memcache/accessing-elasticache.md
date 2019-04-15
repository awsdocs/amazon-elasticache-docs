# Accessing Your Cluster<a name="accessing-elasticache"></a>

Your Amazon ElastiCache instances are designed to be accessed through an Amazon EC2 instance\.

If you launched your ElastiCache instance in an Amazon Virtual Private Cloud \(Amazon VPC\), you can access your ElastiCache instance from an Amazon EC2 instance in the same Amazon VPC\. Or, by using VPC peering, you can access your ElastiCache instance from an Amazon EC2 in a different Amazon VPC\.

If you launched your ElastiCache instance in EC2 Classic, you allow the EC2 instance to access your cluster by granting the Amazon EC2 security group associated with the instance access to your cache security group\. By default, access to a cluster is restricted to the account that launched the cluster\.

**Topics**
+ [Determine the Cluster's Platform](#authorize-access-vpc-or-classic)
+ [Grant Access to Your Cluster](#grant-access)

## Determine the Cluster's Platform<a name="authorize-access-vpc-or-classic"></a>

Before you continue, determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#detecting-platform)\.

### Determining Your Clusters Platform using the ElastiCache Console<a name="authorize-access-vpc-or-classic-console"></a>

The following procedure uses the ElastiCache console to determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

**To determine a cluster's platform using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of your clusters running the Memcached engine, in the left navigation pane, choose **Memcached**\.

1. In the list of clusters, expand the cluster you want to authorize access to by choosing the box to the left of the cluster name\.

1. Locate **Subnet group:**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-SubnetGroup.png)
   + If the **Subnet group** has a name, as shown here, you launched your cluster in EC2\-VPC and should continue at [You Launched Your Cluster into EC2\-VPC](#authorize-access-vpc)\.
   + If there is a dash \(\-\) instead of a **Subnet group** name, you launched your cluster in EC2\-Classic and should continue at [You Launched Your Cluster Running in EC2\-Classic](#authorize-access-ec2-classic)\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#detecting-platform)\.

### Determining Your Clusters Platform using the AWS CLI<a name="authorize-access-vpc-or-classic-cli"></a>

The following procedure uses the AWS CLI to determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

**To determine a cluster's platform using the AWS CLI**

1. Open a command window\.

1. At the command prompt, run the following command\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache describe-cache-clusters \
   --show-cache-cluster-details \
   --cache-cluster-id my-cluster
   ```

   For Windows:

   ```
   aws elasticache describe-cache-clusters ^
   --show-cache-cluster-details ^
   --cache-cluster-id my-cluster
   ```

   JSON output from this command will look something like this\. Some of the output is omitted to save space\.

   ```
   {
   "CacheClusters": [
       {
           "Engine": "memcached", 
            
           "AuthTokenEnabled": false, 
           "CacheParameterGroup": {
               "CacheNodeIdsToReboot": [], 
               "CacheParameterGroupName": "default.memcached1.4",
               
               "ParameterApplyStatus": "in-sync"
           }, 
           "CacheClusterId": "my-cluster-001", 
           "CacheSecurityGroups": [], 
           "NumCacheNodes": 1, 
           "AtRestEncryptionEnabled": false, 
           "CacheClusterCreateTime": "2018-01-16T20:09:34.449Z", 
           "ReplicationGroupId": "my-cluster", 
           "AutoMinorVersionUpgrade": true, 
           "CacheClusterStatus": "available", 
           "PreferredAvailabilityZone": "us-east-2a", 
           "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
           "SecurityGroups": [
               {
                   "Status": "active", 
                   "SecurityGroupId": "sg-e8c03081"
               }
           ], 
           "TransitEncryptionEnabled": false, 
           "CacheSubnetGroupName": "default", 
           "EngineVersion": "3.2.10", 
           "PendingModifiedValues": {}, 
           "PreferredMaintenanceWindow": "sat:05:30-sat:06:30", 
           "CacheNodeType": "cache.t2.medium"
       }
   ]
   }
   ```
   + If there is a value for `CacheSubnetGroupName`, you launched your cluster in EC2\-VPC and should continue at [You Launched Your Cluster into EC2\-VPC](#authorize-access-vpc)\.
   + If there is no value for `CacheSubnetGroupName`, you launched your cluster in EC2\-Classic and should continue at [You Launched Your Cluster Running in EC2\-Classic](#authorize-access-ec2-classic)\.

## Grant Access to Your Cluster<a name="grant-access"></a>

### You Launched Your Cluster into EC2\-VPC<a name="authorize-access-vpc"></a>

If you launched your cluster into an Amazon Virtual Private Cloud \(Amazon VPC\), you can connect to your ElastiCache cluster only from an Amazon EC2 instance that is running in the same Amazon VPC\. In this case, you will need to grant network ingress to the cluster\.

**To grant network ingress from an Amazon VPC security group to a cluster**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Network & Security**, choose **Security Groups**\.

1. From the list of security groups, choose the security group for your Amazon VPC\. Unless you created a security group for ElastiCache use, this security group will be named *default*\.

1. Choose the **Inbound** tab, and then do the following:

   1. Choose **Edit**\.

   1. Choose **Add rule**\.

   1. In the **Type** column, choose **Custom TCP rule**\.

   1. In the **Port range** box, type the port number for your cluster node\. This number must be the same one that you specified when you launched the cluster\. The default port for Memcached is **11211** \.

   1. In the **Source** box, choose **Anywhere** which has the port range \(0\.0\.0\.0/0\) so that any Amazon EC2 instance that you launch within your Amazon VPC can connect to your ElastiCache nodes\.
**Important**  
Opening up the ElastiCache cluster to 0\.0\.0\.0/0 \(Step 4\.e\.\) does not expose the cluster to the Internet because it has no public IP address and therefore cannot be accessed from outside the VPC\. However, the default security group may be applied to other Amazon EC2 instances in the customer’s account, and those instances may have a public IP address\. If they happen to be running something on port 6379, then that service could be exposed unintentionally\. Therefore, we recommend creating a VPC Security Group that will be used exclusively by ElastiCache\. For more information, see [Custom Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#creating-your-own-security-groups)\.

   1. Choose **Save**\.

When you launch an Amazon EC2 instance into your Amazon VPC, that instance will be able to connect to your ElastiCache cluster\.

### You Launched Your Cluster Running in EC2\-Classic<a name="authorize-access-ec2-classic"></a>

If you launched your cluster into EC2\-Classic, to allow an Amazon EC2 instance to access your cluster you will need to grant the Amazon EC2 security group associated with the instance access to your cache security group\.

**To grant an Amazon EC2 security group access to a cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of security groups, from the left navigation pane, choose **Security Groups**\.
**Important**  
If **Security Groups** is not listed in the navigation pane, you launched your cluster in EC2\-VPC rather than EC2\-Classic and should follow the instructions at [You Launched Your Cluster into EC2\-VPC](#authorize-access-vpc)\.

1. Choose the box to the left of **default** security group\.

1. From the list at the bottom of the screen, choose the **EC2 Security Group Name** you want to authorize\.

1. To authorize access, choose **Add**\.

   Amazon EC2 instances that are associated with the security group are now authorized to connect to your ElastiCache cluster\.

To revoke a security group's access, locate the security group in the list of authorized security groups, and then choose **Remove**\.

For more information on ElastiCache Security Groups, see [Security Groups: EC2\-Classic](SecurityGroups.md)\.

### Accessing ElastiCache Resources from Outside AWS<a name="access-from-outside-aws"></a>

Amazon ElastiCache is an AWS service that provides cloud\-based in\-memory key\-value store\. The service is designed to be accessed exclusively from within AWS\. However, if the ElastiCache cluster is hosted inside a VPC, you can use a Network Address Translation \(NAT\) instance to provide outside access\.

#### Requirements<a name="access-from-outside-aws-requirements"></a>

The following requirements must be met for you to access your ElastiCache resources from outside AWS:
+ The cluster must reside within a VPC and be accessed through a Network Address Translation \(NAT\) instance\. There are no exceptions to this requirement\.
+ The NAT instance must be launched in the same VPC as the cluster\.
+ The NAT instance must be launched in a public subnet separate from the cluster\.
+ An Elastic IP Address \(EIP\) must be associated with the NAT instance\. The port forwarding feature of iptables is used to forward a port on the NAT instance to the cache node port within the VPC\.

#### Considerations<a name="access-from-outside-aws-considerations"></a>

The following considerations should be kept in mind when accessing your ElastiCache resources from outside ElastiCache\.
+ Clients connect to the EIP and cache port of the NAT instance\. Port forwarding on the NAT instance forwards traffic to the appropriate cache cluster node\.
+ If a cluster node is added or replaced, the iptables rules need to be updated to reflect this change\.

#### Limitations<a name="access-from-outside-aws-limitations"></a>

This approach should be used for testing and development purposes only\. It is not recommended for production use due to the following limitations:
+ The NAT instance is acting as a proxy between clients and multiple clusters\. The addition of a proxy impacts the performance of the cache cluster\. The impact increases with number of cache clusters you are accessing through the NAT instance\.
+ The traffic from clients to the NAT instance is unencrypted\. Therefore, you should avoid sending sensitive data via the NAT instance\.
+ The NAT instance adds the overhead of maintaining another instance\.
+ The NAT instance serves as a single point of failure\. For information about how to set up high availability NAT on VPC, see [High Availability for Amazon VPC NAT Instances: An Example](https://aws.amazon.com/articles/2781451301784570)\.

#### How to Access ElastiCache Resources from Outside AWS<a name="access-from-outside-aws-how-to"></a>

The following procedure demonstrates how to connect to your ElastiCache resources using a NAT instance\.

These steps assume the following:
+ You are accessing a Memcached cluster with:
  + IP address – *10\.0\.1\.230*
  + Default Memcached port – *11211*
  + Security group – *sg\-bd56b7da*
+ Your trusted client has the IP address *198\.51\.100\.27*\.
+ Your NAT instance has the Elastic IP Address *203\.0\.113\.73*\.
+ Your NAT instance has security group *sg\-ce56b7a9*\.

**To connect to your ElastiCache resources using a NAT instance**

1. Create a NAT instance in the same VPC as your cache cluster but in a public subnet\.

   By default, the VPC wizard will launch a *cache\.m1\.small* node type\. You should select a node size based on your needs\. You must use EC2 NAT AMI to be able to access ElastiCache from outside AWS\.

   For information about creating a NAT instance, see [NAT Instances](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html) in the AWS VPC User Guide\.

1. Create security group rules for the cache cluster and NAT instance\.

   The NAT instance security group should have the following rules:
   + Two inbound rules
     + One to allow TCP connections from trusted clients to each cache port forwarded from the NAT instance \(11211 \- 11213\)\.
     + A second to allow SSH access to trusted clients\.  
**NAT Instance Security Group \- Inbound Rules**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/accessing-elasticache.html)
   + An outbound rule to allow TCP connections to cache port \(11211\)\.  
**NAT Instance Security Group \- Outbound Rule**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/accessing-elasticache.html)
   + An inbound rule for the cluster's security group that allows TCP connections from the NAT instance to the cache port \(11211\)\.  
**Cluster Instance Security Group \- Inbound Rule**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/accessing-elasticache.html)

1. Validate the rules\.
   + Confirm that the trusted client is able to SSH to the NAT instance\.
   + Confirm that the trusted client is able to connect to the cluster from the NAT instance\.

1. Add an iptables rule to the NAT instance\.

   An iptables rule must be added to the NAT table for each node in the cluster to forward the cache port from the NAT instance to the cluster node\. An example might look like the following:

   ```
   iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 11211 -j DNAT --to 10.0.1.230:11211
   ```

   The port number must be unique for each node in the cluster\. For example, if working with a three node Memcached cluster using ports 11211 \- 11213, the rules would look like the following:

   ```
   iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 11211 -j DNAT --to 10.0.1.230:11211
   iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 11212 -j DNAT --to 10.0.1.231:11211
   iptables -t nat -A PREROUTING -i eth0 -p tcp --dport 11213 -j DNAT --to 10.0.1.232:11211
   ```

1. Confirm that the trusted client is able to connect to the cluster\.

   The trusted client should connect to the EIP associated with the NAT instance and the cluster port corresponding to the appropriate cluster node\. For example, the connection string for PHP might look like the following:

   ```
   $memcached->connect( '203.0.113.73', 11211 );
   $memcached->connect( '203.0.113.73', 11212 );
   $memcached->connect( '203.0.113.73', 11213 );
   ```

   A telnet client can also be used to verify the connection\. For example:

   ```
   telnet 203.0.113.73 11211
   telnet 203.0.113.73 11212
   telnet 203.0.113.73 11213
   ```

1. Save the iptables configuration\.

   Save the rules after you test and verify them\. If you are using a Redhat\-based Linux distribution \(like Amazon Linux\), run the following command:

   ```
   service iptables save
   ```

#### Related topics<a name="access-from-outside-aws-see-also"></a>

The following topics may be of additional interest\.
+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)
+ [Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center](elasticache-vpc-accessing.md#elasticache-vpc-accessing-data-center)
+ [NAT Instances](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html)
+ [Configuring ElastiCache Clients](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/ClientConfig.html)
+ [High Availability for Amazon VPC NAT Instances: An Example](https://aws.amazon.com/articles/2781451301784570)