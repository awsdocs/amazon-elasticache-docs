# Managing the Service Updates<a name="managing-updates"></a>

ElastiCache service updates are released on a regular basis\. If you have one or more qualifying clusters for those service updates, you will receive notifications through email, SNS, the Personal Health Dashboard \(PHD\), and CloudWatch events\. The updates are also displayed on the **Service Updates** page on the ElastiCache console\. This dashboard view allows you to view all the service updates and their status with regard to your ElastiCache Redis fleet\. It's an audit log that you can use when reviewing your Redis fleet for service updates\.

**Note**  
 This is important when reviewing your fleet for compliance\. For more information, see [Self\-Service Security Updates for Compliance](elasticache-compliance.md#elasticache-compliance-self-service)\.

However, you control when to apply an update, regardless of the recommendation\. At a minimum, we strongly recommend that you apply any updates of type **security** to ensure that your Redis clusters are always up\-to\-date with current security patches\. To view the up\-to\-date status of all your Redis clusters, you can choose **Service Update Status**\. It also shows the clusters for which the update is not applicable\. In addition, if you discover that any updates you've applied to Redis clusters are exceeding their estimated update time and interrupt your business flows, you can stop them and re\-apply them at a time that better suits your business needs\. 

For more information, see [Amazon ElastiCache Maintenance Help Page](https://aws.amazon.com/elasticache/elasticache-maintenance/)\.

The following sections explore these options in detail:

**Topics**
+ [Applying the Self\-Service Updates](applying-updates.md)
+ [Stopping the Self\-Service Updates](stopping-self-service-updates.md)