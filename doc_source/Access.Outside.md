# Accessing ElastiCache Resources from Outside AWS<a name="Access.Outside"></a>

Amazon ElastiCache is an AWS service that provides cloud\-based in\-memory key\-value store\. On the back end it uses either the Memcached or Redis engine\. The service is designed to be accessed exclusively from within AWS\. However, if the ElastiCache cluster is hosted inside an Amazon VPC, there are a number of ways to access a cluster from outside AWS, one of which is using a Network Address Translation \(NAT\) instance to provide outside access, as described in this topic\. For other patterns, see [Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center](elasticache-vpc-accessing.md#elasticache-vpc-accessing-data-center)\.


+ [Requirements](#Access.Outside.Requirements)
+ [Considerations](#Access.Outside.Considerations)
+ [Limitations](#Access.Outside.Limitations)
+ [How to Access ElastiCache Resources from Outside AWS](#Access.Outside.HowTo)
+ [Related topics](#Access.Outside.SeeAlso)

## Requirements<a name="Access.Outside.Requirements"></a>

The following requirements must be met for you to access your ElastiCache resources from outside AWS using a NAT instance\. For other ways of accessing ElastiCache from outside AWS see, [Accessing an ElastiCache Cluster from an Application Running in a Customer's Data Center](elasticache-vpc-accessing.md#elasticache-vpc-accessing-data-center)\.

+ The cluster must reside within an Amazon VPC and be accessed through a Network Address Translation \(NAT\) instance\.

+ The NAT instance must be launched in the same Amazon VPC as the cluster\.

+ The NAT instance must be launched in a public subnet separate from the cluster\.

+ An Elastic IP Address \(EIP\) must be associated with the NAT instance\. The port forwarding feature of iptables is used to forward a port on the NAT instance to the cache node port within the Amazon VPC\.

## Considerations<a name="Access.Outside.Considerations"></a>

The following considerations should be kept in mind when accessing your ElastiCache resources from outside ElastiCache\.

+ Clients connect to the EIP and cache port of the NAT instance\. Port forwarding on the NAT instance forwards traffic to the appropriate cache cluster node\.

+ If a cluster node is added or replaced, the iptables rules need to be updated to reflect this change\.

## Limitations<a name="Access.Outside.Limitations"></a>

This approach should be used for testing and development purposes only\. It is not recommended for production use due to the following limitations:

+ The NAT instance is acting as a proxy between clients and multiple clusters\. The addition of a proxy impacts the performance of the cache cluster\. The impact increases with number of cache clusters you are accessing through the NAT instance\.

+ The traffic from clients to the NAT instance is unencrypted\. Therefore, you should avoid sending sensitive data via the NAT instance\.

+ The NAT instance adds the overhead of maintaining another instance\.

+ The NAT instance serves as a single point of failure\. For information about how to set up high availability NAT on VPC, see [High Availability for Amazon VPC NAT Instances: An Example](https://aws.amazon.com/articles/2781451301784570)\.

## How to Access ElastiCache Resources from Outside AWS<a name="Access.Outside.HowTo"></a>

The following procedure demonstrates how to connect to your ElastiCache resources using a NAT instance\.

**Tip**  
You can modify the following process to work for any Amazon ElastiCache cluster\. Just substitute the cluster's port number for the port numbers in the example\.

These steps assume the following:

+ You are accessing a Memcached cluster with the IP address *10\.0\.1\.230*, the default Memcached port *11211*, and security group *sg\-bd56b7da*\.

+ Your trusted client has the IP address *198\.51\.100\.27*\.

+ Your NAT instance has the Elastic IP Address *203\.0\.113\.73*\.

+ Your NAT instance has security group *sg\-ce56b7a9*\.

When you finish creating your NAT instance using the following steps, the following should be true\.

+ IP forwarding is enabled for the NAT instance\. The following command can be used to confirm this\.

  ```
  cat /proc/sys/net/ipv4/ip_forward
  ```

+ Masquerading is enabled\. The following command can be used to enable masquerading\.

  ```
  iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
  ```

**To connect to your ElastiCache resources using a NAT instance**

1. Create a NAT instance in the same VPC as your cache cluster but in a public subnet\.

   By default, the VPC wizard will launch a *cache\.m1\.small* node type\. You should choose a node size based on your needs\.

   For information about creating a NAT instance, see [NAT Instances](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html) in the AWS VPC User Guide\.

1. Create security group rules for the cache cluster and NAT instance\.

   The NAT instance security group should have the following rules:

   + Two inbound rules

     + One to allow TCP connections from trusted clients to each cache port forwarded from the NAT instance \(11211 \- 11213\)\.

     + A second to allow SSH access to trusted clients\.  
**NAT Instance Security Group \- Inbound Rules**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Access.Outside.html)

   + An outbound rule to allow TCP connections to each forwarded cache port \(11211\-11213\)\.  
**NAT Instance Security Group \- Outbound Rule**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Access.Outside.html)

   + An inbound rule for the cluster's security group that allows TCP connections from the NAT instance to the cache port on each instance in the cluster \(11211\-11213\)\.  
**Cache Cluster Security Group \- Inbound Rule**    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/Access.Outside.html)

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

## Related topics<a name="Access.Outside.SeeAlso"></a>

+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)

+ [NAT Instances](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_NAT_Instance.html)

+ [Configuring ElastiCache Clients](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/ClientConfig.html)

+ [High Availability for Amazon VPC NAT Instances: An Example](https://aws.amazon.com/articles/2781451301784570)