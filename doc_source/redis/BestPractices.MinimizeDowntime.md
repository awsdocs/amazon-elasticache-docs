# Best Pratices: Minimizing Downtime During Maintenance<a name="BestPractices.MinimizeDowntime"></a>

Cluster mode configuration has the best availability during managed or unmanaged operations\. It is always recommended to use a cluster mode supported client which connects to the cluster discovery endpoint\. For cluster mode disabled, it is recommended to always use the primary endpoint for all the write operations\. 

For read activity, applications can also connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\. 

If auto\-failover is enabled in the cluster, the primary node may change, therefore, the application should confirm the role of the node and update all the read endpoints to ensure that you aren't causing a major load on the master\. With auto failover disabled, the role of the node will not change; however the downtime in managed or unmanaged operations is higher as compared to clusters with auto failover enabled\.

 Avoid directing read requests to read replicas only\. If you configure your client to direct read requests to read replicas only, ensure that you have at least two read replicas to avoid any read interruption during maintenance\. 