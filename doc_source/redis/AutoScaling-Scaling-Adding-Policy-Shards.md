# Adding a scaling policy<a name="AutoScaling-Scaling-Adding-Policy-Shards"></a>

You can add a scaling policy using the AWS Management Console\. 

**To add an Auto Scaling policy to an ElastiCache for Redis cluster**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. In the navigation pane, choose **Redis**\. 

1. Choose the cluster that you want to add a policy to\. 

1. Choose the **Manage Auto Scaling policies** from the **Actions** dropdown\. 

1. Choose the **Auto Scaling policies** tab\. 

1. In the **Add Scaling policy** dialog box, choose **Dynamic scaling**\. 

1. For **Policy name** enter a policy name\. 

1. For **Scalable Dimension** choose **shards**\. 

1. For the target metric, choose one of the following:
   + **Primary CPU Utilization** to create a policy based on the average CPU utilization\. 
   + **Memory** to create a policy based on the average database memory\. 

1. For the target value, choose one of the following:
   + If you chose **Primary CPU Utilization**, type the percentage of CPU utilization that you want to maintain on Elasticache shards\. This value must be greater than or equal to 35 and less than or equal to 70\. 
   + If you chose **Memory** to create a policy based on the average database memory\. type the percentage of Redis memory that you want to maintain on ElastiCache for Redis shards\. This value must be greater than or equal to 35 and less than or equal to 70\. 

   Cluster shards are added or removed to keep the metric close to the specified value\. 

1. \(Optional\) Scale\-in or scale\-out cooldown periods are not supported from the console\. Use the AWS CLI to modify the cooldown values\. 

1. For **Minimum capacity**, type the minimum number of shards that the ElastiCache for Redis Auto Scaling policy is required to maintain\. 

1. For **Maximum capacity**, type the maximum number of shards that the ElastiCache for Redis Auto Scaling policy is required to maintain\. This value must be less than or equal to 250\.

1. Choose **Add policy**\.