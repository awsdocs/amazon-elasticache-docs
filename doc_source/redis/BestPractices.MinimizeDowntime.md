# Best practices: Minimizing downtime during maintenance<a name="BestPractices.MinimizeDowntime"></a>

Cluster mode configuration has the best availability during managed or unmanaged operations\. We recommend that you use a cluster mode supported client that connects to the cluster discovery endpoint\. For cluster mode disabled, we recommend that you use the primary endpoint for all write operations\. 

For read activity, applications can also connect to any node in the cluster\. Unlike the primary endpoint, node endpoints resolve to specific endpoints\. If you make a change in your cluster, such as adding or deleting a replica, you must update the node endpoints in your application\. 

If autofailover is enabled in the cluster, the primary node might change\. Therefore, the application should confirm the role of the node and update all the read endpoints\. Doing this helps ensure that you aren't causing a major load on the primary\. With autofailover disabled, the role of the node doesn't change\. However, the downtime in managed or unmanaged operations is higher as compared to clusters with auto failover enabled\.

 Avoid directing read requests to read replicas only\. If you configure your client to direct read requests to read replicas only, ensure that you have at least two read replicas to avoid any read interruption during maintenance\. 