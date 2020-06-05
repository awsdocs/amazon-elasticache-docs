# Mitigating Failure Issues When Using Redis AOF<a name="BestPractices.AOF"></a>

When planning your Amazon ElastiCache implementation, you should plan so that failures have the least impact possible\.

You enable AOF because an AOF file is useful in recovery scenarios\. In case of a node restart or service crash, Redis replays the updates from an AOF file, thereby recovering the data lost due to the restart or crash\.

**Warning**  
AOF cannot protect against all failure scenarios\. For example, if a node fails due to a hardware fault in an underlying physical server, ElastiCache provisions a new node on a different server\. In this case, the AOF file is no longer available and cannot be used to recover the data\. Thus, Redis restarts with a cold cache\.

## Enabling Redis Multi\-AZ as a Better Approach to Fault Tolerance<a name="BestPractices.AOF.MultiAZ"></a>

If you are enabling AOF to protect against data loss, consider using a replication group with Multi\-AZ enabled instead of AOF\. When using a Redis replication group, if a replica fails, it is automatically replaced and synchronized with the primary cluster\. If Multi\-AZ is enabled on a Redis replication group and the primary fails, it fails over to a read replica\. Generally, this functionality is much faster than rebuilding the primary from an AOF file\. For greater reliability and faster recovery, we recommend that you create a replication group with one or more read replicas in different Availability Zones and enable Multi\-AZ instead of using AOF\. Because there is no need for AOF in this scenario, ElastiCache disables AOF on Multi\-AZ replication groups\.

For more information, see the following topics:
+ [Mitigating Failures](FaultTolerance.md)
+ [High Availability Using Replication Groups](Replication.md)
+ [Minimizing Downtime in ElastiCache for Redis with Multi\-AZ](AutoFailover.md)