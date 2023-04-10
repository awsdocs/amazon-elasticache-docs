# Adding a scaling policy<a name="AutoScaling-Adding-Policy-Replicas"></a>

You can add a scaling policy using the AWS Management Console\. 

**Adding a scaling policy using the AWS Management Console**

To add an auto scaling policy to an ElastiCache for Redis

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\. 

1. Choose the cluster that you want to add a policy to \(choose the cluster name and not the button to its left\)\. 

1. Choose the **Auto Scaling policies** tab\. 

1. Choose **add dynamic scaling**\. 

1. Under **Scaling policies**, choose **Add dynamic scaling**\.

1. For **Policy Name**, enter the policy name\. 

1. For **Scalable Dimension**, select **Replicas** from dialog box\. 

1. For the target value, type the Avg percentage of CPU utilization that you want to maintain on Elasticache Replicas\. This value must be >=35 and <=70\. Cluster replicas are added or removed to keep the metric close to the specified value\.

1. \(Optional\) scale\-in or scale\-out cooldown periods are not supported from the Console\. Use the AWS CLI to modify the cool down values\. 

1. For **Minimum capacity**, type the minimum number of replicas that the ElastiCache for Redis Auto Scaling policy is required to maintain\. 

1. For **Maximum capacity**, type the maximum number of replicas the ElastiCache for Redis Auto Scaling policy is required to maintain\. This value must be >=5\. 

1. Choose **Create**\.