# Stopping the Service Updates Using the Console<a name="stopping-updates-console-memcached"></a>

You can interrupt a service update using the console\. The following demonstrates how to do this:
+ After a service update has progressed on a selected Memcached cluster, the ElastiCache console displays the **View/Stop Update** tab at the top of the dashboard\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ssp-view-stop.png)
+ To interrupt the update, choose **Stop Update**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ssp-stop-1.png)
+ When you stop the update, choose the Memcached cluster and examine the status\. It will revert to a **Stopping** status and eventually a **Stopped** status\.