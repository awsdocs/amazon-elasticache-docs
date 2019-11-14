# Managing the Service Updates<a name="managing-updates"></a>

ElastiCache service updates are released on a regular basis\. If you have one or more qualifying clusters for those service updates, you receive notifications through email, SNS, the Personal Health Dashboard \(PHD\), and Amazon CloudWatch events\. The updates are also displayed on the **Service Updates** page on the ElastiCache console\. This dashboard view enables you to view all the service updates and their status for to your ElastiCache Redis fleet\. It's an audit log that you can use when reviewing your Redis fleet for service updates\.

**Note**  
Using this log can prove important when reviewing your fleet for compliance\. For more information, see [Self\-Service Security Updates for Compliance](elasticache-compliance.md#elasticache-compliance-self-service)\.

However, you control when to apply an update, regardless of the recommendation\. At a minimum, we strongly recommend that you apply any updates of type **security** to ensure that your Redis clusters are always up\-to\-date with current security patches\. To view the up\-to\-date status of all your Redis clusters, you can choose **Service Update Status**\. This view also shows the clusters for which the update is not applicable\. In addition, you might find updates that you applied to Redis clusters exceed their estimated update time and interrupt your business flows\. In this case, you can stop them and reapply them at a time that better suits your business needs\. 

For more information, see [Amazon ElastiCache Maintenance Help Page](https://aws.amazon.com/elasticache/elasticache-maintenance/)\.

The following sections explore these options in detail\.

**Topics**
+ [Applying the Self\-Service Updates](applying-updates.md)
+ [Stopping the Self\-Service Updates](stopping-self-service-updates.md)