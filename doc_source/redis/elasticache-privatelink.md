# Amazon ElastiCache API and interface VPC endpoints \(AWS PrivateLink\)<a name="elasticache-privatelink"></a>

You can establish a private connection between your VPC and Amazon ElastiCache API endpoints by creating an *interface VPC endpoint*\. Interface endpoints are powered by [AWS PrivateLink](https://aws.amazon.com/privatelink)\. AWS PrivateLink allows you to privately access Amazon ElastiCache API operations without an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection\. 

Instances in your VPC don't need public IP addresses to communicate with Amazon ElastiCache API endpoints\. Your instances also don't need public IP addresses to use any of the available ElastiCache API operations\. Traffic between your VPC and Amazon ElastiCache doesn't leave the Amazon network\. Each interface endpoint is represented by one or more elastic network interfaces in your subnets\. For more information on elastic network interfaces, see [Elastic network interfaces](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-eni.html) in the *Amazon EC2 User Guide*\. 
+ For more information about VPC endpoints, see [Interface VPC endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\.
+ For more information about ElastiCache API operations, see [ElastiCache API operations](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/Welcome.html)\. 

After you create an interface VPC endpoint, if you enable [private DNS](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#vpce-private-dns) hostnames for the endpoint, the default ElastiCache endpoint \(https://elasticache\.*Region*\.amazonaws\.com\) resolves to your VPC endpoint\. If you do not enable private DNS hostnames, Amazon VPC provides a DNS endpoint name that you can use in the following format:

```
VPC_Endpoint_ID.elasticache.Region.vpce.amazonaws.com
```

For more information, see [Interface VPC Endpoints \(AWS PrivateLink\)](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) in the *Amazon VPC User Guide*\. ElastiCache supports making calls to all of its [API Actions](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_Operations.html) inside your VPC\. 

**Note**  
Private DNS hostnames can be enabled for only one VPC endpoint in the VPC\. If you want to create an additional VPC endpoint then private DNS hostname should be disabled for it\.

## Considerations for VPC endpoints<a name="elasticache-privatelink-considerations"></a>

Before you set up an interface VPC endpoint for Amazon ElastiCache API endpoints, ensure that you review [Interface endpoint properties and limitations](https://docs.aws.amazon.com/vpc/latest/privatelink/endpoint-services-overview.html) in the *Amazon VPC User Guide*\. All ElastiCache API operations relevant to managing Amazon ElastiCache resources are available from your VPC using AWS PrivateLink\. 

VPC endpoint policies are supported for ElastiCache API endpoints\. By default, full access to ElastiCache API operations is allowed through the endpoint\. For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\. 

## Creating an interface VPC endpoint for the ElastiCache API<a name="elasticache-privatelink-create-vpc-endpoint"></a>

You can create a VPC endpoint for the Amazon ElastiCache API using either the Amazon VPC console or the AWS CLI\. For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/privatelink/create-endpoint-service.html) in the *Amazon VPC User Guide*\.

 After you create an interface VPC endpoint, you can enable private DNS hostnames for the endpoint\. When you do, the default Amazon ElastiCache endpoint \(https://elasticache\.*Region*\.amazonaws\.com\) resolves to your VPC endpoint\. For the China \(Beijing\) and China \(Ningxia\) AWS Regions, you can make API requests with the VPC endpoint by using `elasticache.cn-north-1.amazonaws.com.cn` for Beijing and `elasticache.cn-northwest-1.amazonaws.com.cn` for Ningxia\. For more information, see [Accessing a service through an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#access-service-though-endpoint) in the *Amazon VPC User Guide*\. 

## Creating a VPC endpoint policy for the Amazon ElastiCache API<a name="elasticache-privatelink-policy"></a>

You can attach an endpoint policy to your VPC endpoint that controls access to the ElastiCache API\. The policy specifies the following:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resources on which actions can be performed\.

For more information, see [Controlling access to services with VPC endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html) in the *Amazon VPC User Guide*\.

**Example VPC endpoint policy for ElastiCache API actions**  
The following is an example of an endpoint policy for the ElastiCache API\. When attached to an endpoint, this policy grants access to the listed ElastiCache API actions for all principals on all resources\.  

```
{
	"Statement": [{
		"Principal": "*",
		"Effect": "Allow",
		"Action": [
			"elasticache:CreateCacheCluster",
			"elasticache:ModifyCacheCluster",
			"elasticache:CreateSnapshot"
		],
		"Resource": "*"
	}]
}
```

**Example VPC endpoint policy that denies all access from a specified AWS account**  
The following VPC endpoint policy denies AWS account *123456789012* all access to resources using the endpoint\. The policy allows all actions from other accounts\.  

```
{
	"Statement": [{
			"Action": "*",
			"Effect": "Allow",
			"Resource": "*",
			"Principal": "*"
		},
		{
			"Action": "*",
			"Effect": "Deny",
			"Resource": "*",
			"Principal": {
				"AWS": [
					"123456789012"
				]
			}
		}
	]
}
```