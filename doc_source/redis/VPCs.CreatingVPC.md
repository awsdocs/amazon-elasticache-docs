# Creating a Virtual Private Cloud \(VPC\)<a name="VPCs.CreatingVPC"></a>

In this example, you create an Amazon VPC with a private subnet for each Availability Zone\.

## Creating an Amazon VPC \(Console\)<a name="VPCs.CreatingVPC.CON"></a>

**To create an ElastiCache cluster inside an Amazon Virtual Private Cloud**

1. Sign in to the AWS Management Console, and open the Amazon VPC console at [https://console\.aws\.amazon\.com/vpc/](https://console.aws.amazon.com/vpc/)\.

1. Create a new Amazon VPC by using the Amazon Virtual Private Cloud wizard:

   1. In the navigation list, choose **VPC Dashboard**\.

   1. Choose **Start VPC Wizard**\.

   1. In the Amazon VPC wizard, choose **VPC with Public and Private Subnets**, and then choose **Next**\.

   1. On the **VPC with Public and Private Subnets** page, keep the default options, and then choose **Create VPC**\.

   1. In the confirmation message that appears, choose **Close**\.

1. Confirm that there are two subnets in your Amazon VPC, a public subnet and a private subnet\. These subnets are created automatically\.

   1. In the navigation list, choose **Subnets**\.

   1. In the list of subnets, find the two subnets that are in your Amazon VPC:  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/vpc-01.png)

      The public subnet will have one fewer available IP address, because the wizard creates an Amazon EC2 NAT instance and an Elastic IP address \(for which Amazon EC2 rates apply\) for outbound communication to the Internet from your private subnet\.
**Tip**  
Make a note of your two subnet identifiers, and which is public and private\. You will need this information later when you launch your cache clusters and add an Amazon EC2 instance to your Amazon VPC\.

1. Create an Amazon VPC security group\. You will use this group for your cache cluster and your Amazon EC2 instance\.

   1. In the navigation pane of the Amazon VPC Management console, choose **Security Groups**\.

   1. Choose **Create Security Group**\.

   1. Type a name and a description for your security group in the corresponding boxes\. In the **VPC** box, choose the identifier for your Amazon VPC\.  
![\[Image: Create Security Group screen\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/images/vpc-02.png)

   1. When the settings are as you want them, choose **Yes, Create**\.

1. Define a network ingress rule for your security group\. This rule will allow you to connect to your Amazon EC2 instance using Secure Shell \(SSH\)\.

   1. In the navigation list, choose **Security Groups**\.

   1. Find your security group in the list, and then choose it\. 

   1. Under **Security Group**, choose the **Inbound** tab\. In the **Create a new rule** box, choose **SSH**, and then choose **Add Rule**\.

   1. Set the following values for your new inbound rule to allow HTTP access: 
      + Type: HTTP
      + Source: 0\.0\.0\.0/0

      Choose **Apply Rule Changes**\.

Now you are ready to create a cache subnet group and launch a cache cluster in your Amazon VPC\. 
+ [Creating a subnet group](SubnetGroups.Creating.md)
+ [Creating a Redis \(cluster mode disabled\) \(Console\)](Clusters.Create.CON.Redis.md)\. 