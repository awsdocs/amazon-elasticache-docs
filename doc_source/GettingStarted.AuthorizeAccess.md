# Step 4: Authorize Access<a name="GettingStarted.AuthorizeAccess"></a>

 This section assumes that you are familiar with launching and connecting to Amazon EC2 instances\. For more information, go to the *[Amazon EC2 Getting Started Guide](http://docs.aws.amazon.com/AWSEC2/latest/GettingStartedGuide/)*\. 

All ElastiCache clusters are designed to be accessed from an Amazon EC2 instance\. The most common scenario is to access an ElastiCache cluster from an Amazon EC2 instance in the same Amazon Virtual Private Cloud \(Amazon VPC\)\. This is the scenario covered in this topic\. For information on accessing your ElastiCache cluster from a different Amazon VPC, a different region, or even your corporate network, see:

+ [Access Patterns for Accessing an ElastiCache Cluster in an Amazon VPC](elasticache-vpc-accessing.md)

+ [Accessing ElastiCache Resources from Outside AWS](Access.Outside.md)

By default, network access to your cluster is limited to the user account that was used to launch it\. Before you can connect to a cluster from an EC2 instance, you must authorize the EC2 instance to access the cluster\. The steps required depend upon whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

Steps to authorize access

## Step 4\.1: Determine Environment<a name="getting-started-authorize-access-vpc-or-classic"></a>

Before you continue, determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html#detecting-platform)\.

### Determining Your Clusters Platform using the Amazon EC2 Console<a name="getting-started-authorize-access-vpc-or-classic-console"></a>

The following procedure uses the Amazon EC2 console to determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

**To determine a cluster's platform using the Amazon EC2 console**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. Locate **Supported Platforms** in the upper\-right corner\.

   Under **Supported Platforms**, you will see either only **VPC** or both **EC2** and **VPC**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-EC2-SupportedPlatforms.png)

   + If you see only **VPC** \(as in the left example\), continue at [You Launched Your Cluster into EC2\-VPC](#GettingStarted.AuthorizeAccess.VPC)\.

   + If you see both **EC2** and **VPC** \(as in the right example\), continue at [You Launched Your Cluster Running in EC2\-Classic](#GettingStarted.AuthorizeAccess.Classic)\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html#detecting-platform)\.

### Determining Your Clusters Platform using the AWS CLI<a name="getting-started-authorize-access-vpc-or-classic-cli"></a>

The following procedure uses the AWS CLI to determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

**To determine a cluster's platform using the AWS CLI**

1. Open a command window\.

1. At the command prompt, run the following command\.

   ```
   aws ec2 describe-account-attributes
   ```

   JSON output from this command will look something like this\.

   ```
   {
       "AccountAttributes": [
           {
               "AttributeName": "supported-platforms", 
               "AttributeValues": [
                   {
                       "AttributeValue": "VPC"
                   }
               ]
           }
           
           **** some output omitted for brevity ****
           
       ]
   }
   ```

   + If the `AttributeValue` for `supported-platforms` is **VPC** \(as in the preceeding example\), continue at [You Launched Your Cluster into EC2\-VPC](#GettingStarted.AuthorizeAccess.VPC)\.

   + If there are two attribute values, **EC2** and **VPC**, continue at [You Launched Your Cluster Running in EC2\-Classic](#GettingStarted.AuthorizeAccess.Classic)\.

## Step 4\.2: Grant Access<a name="getting-started-grant-access"></a>

### You Launched Your Cluster into EC2\-VPC<a name="GettingStarted.AuthorizeAccess.VPC"></a>

If you launched your cluster into an Amazon Virtual Private Cloud \(Amazon VPC\), you can connect to your ElastiCache cluster only from an Amazon EC2 instance that is running in the same Amazon VPC\. In this case, you will need to grant network ingress to the cluster\.

**To grant network ingress from an Amazon VPC security group to a cluster**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Network & Security**, choose **Security Groups**\.

1. From the list of security groups, choose the security group for your Amazon VPC\. Unless you created a security group for ElastiCache use, this security group will be named *default*\.

1. Choose the **Inbound** tab, and then do the following:

   1. Choose **Edit**\.

   1. Choose **Add rule**\.

   1. In the **Type** column, choose **Custom TCP rule**\.

   1. In the **Port range** box, type the port number for your cluster node\. This number must be the same one that you specified when you launched the cluster\. The default ports are as follows:

      + Memcached: port 11211

      + Redis: port 6379

   1. In the **Source** box, choose **Anywhere** which has the port range \(0\.0\.0\.0/0\) so that any Amazon EC2 instance that you launch within your Amazon VPC can connect to your ElastiCache nodes\.
**Important**  
Opening up the ElastiCache cluster to 0\.0\.0\.0/0 \(Step 4\.e\.\) does not expose the cluster to the Internet because it has no public IP address and therefore cannot be accessed from outside the VPC\. However, the default security group may be applied to other Amazon EC2 instances in the customer’s account, and those instances may have a public IP address\. If they happen to be running something on port 6379, then that service could be exposed unintentionally\. Therefore, we recommend creating a VPC Security Group that will be used exclusively by ElastiCache\. For more information, see [Custom Security Groups](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#creating-your-own-security-groups)\.

   1. Choose **Save**\.

When you launch an Amazon EC2 instance into your Amazon VPC, that instance will be able to connect to your ElastiCache cluster\.

### You Launched Your Cluster Running in EC2\-Classic<a name="GettingStarted.AuthorizeAccess.Classic"></a>

If you launched your cluster into EC2\-Classic, to allow an Amazon EC2 instance to access your cluster you will need to grant the Amazon EC2 security group associated with the instance access to your cache security group\.

**To grant an Amazon EC2 security group access to a cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of security groups, from the left navigation pane, choose **Security Groups**\.

1. Choose the box to the left of **default** security group\.

1. From the list at the bottom of the screen, choose the **EC2 Security Group Name** you want to authorize\.

1. To authorize access, choose **Add**\.

   Amazon EC2 instances that are associated with the security group are now authorized to connect to your ElastiCache cluster\.

To revoke a security group's access, locate the security group in the list of authorized security groups, and then choose **Remove**\.