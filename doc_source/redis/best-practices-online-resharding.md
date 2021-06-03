# Best practices: Online cluster resizing<a name="best-practices-online-resharding"></a>

*Resharding * involves adding and removing shards or nodes to your cluster and redistributing key spaces\. As a result, multiple things have an impact on the resharding operation, such as the load on the cluster, memory utilization, and overall size of data\. For the best experience, we recommend that you follow overall cluster best practices for uniform workload pattern distribution\. In addition, we recommend taking the following steps\.

Before initiating resharding, we recommend the following:
+ **Test your application** – Test your application behavior during resharding in a staging environment if possible\.
+ **Get early notification for scaling issues** – Resharding is a compute\-intensive operation\. Because of this , we recommend keeping CPU utilization under 80 percent on multicore instances and less than 50 percent on single core instances during resharding\. Monitor ElastiCache for Redis metrics and initiate resharding before your application starts observing scaling issues\. Useful metrics to track are `CPUUtilization`, `NetworkBytesIn`, `NetworkBytesOut`, `CurrConnections`, `NewConnections`, `FreeableMemory`, `SwapUsage`, and `BytesUsedForCacheItems`\.
+ **Ensure sufficient free memory is available before scaling in** – If you're scaling in, ensure that free memory available on the shards to be retained is at least 1\.5 times the memory used on the shards you plan to remove\.
+ **Initiate resharding during off\-peak hours** – This practice helps to reduce the latency and throughput impact on the client during the resharding operation\. It also helps to complete resharding faster as more resources can be used for slot redistribution\.
+ **Review client timeout behavior** – Some clients might observe higher latency during online cluster resizing\. Configuring your client library with a higher timeout can help by giving the system time to connect even under higher load conditions on server\. In some cases, you might open a large number of connections to the server\. In these cases, consider adding exponential backoff to reconnect logic\. Doing this can help prevent a burst of new connections hitting the server at the same time\.

During resharding, we recommend the following:
+ **Avoid expensive commands** – Avoid running any computationally and I/O intensive operations, such as the `KEYS` and `SMEMBERS` commands\. We suggest this approach because these operations increase the load on the cluster and have an impact on the performance of the cluster\. Instead, use the `SCAN` and `SSCAN` commands\.
+ **Follow Lua best practices** – Avoid long running Lua scripts, and always declare keys used in Lua scripts up front\. We recommend this approach to determine that the Lua script is not using cross slot commands\. Ensure that the keys used in Lua scripts belong to the same slot\.

After resharding, note the following:
+ Scale\-in might be partially successful if insufficient memory is available on target shards\. If such a result occurs, review available memory and retry the operation, if necessary\.
+ Slots with large items are not migrated\. In particular, slots with items larger than 256 MB post\-serialization are not migrated\.
+ The `BRPOPLPUSH` command is not supported if it operates on the slot being migrated\. `FLUSHALL` and `FLUSHDB` commands are not supported inside Lua scripts during a resharding operation\.