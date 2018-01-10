# Engines and Versions<a name="SelectEngine"></a>

Amazon ElastiCache supports the Memcached and Redis cache engines\. Each engine provides some advantages\. Use the information in this topic to help you choose the engine and version that best meets your requirements\.

**Important**  
After you create a cache cluster or replication group, you can upgrade to a newer engine version \(see [Upgrading Engine Versions](VersionManagement.md)\), but you cannot downgrade to an older engine version\. If you want to use an older engine version, you must delete the existing cache cluster or replication group and create it anew with the earlier engine version\.


+ [Choosing an Engine: Memcached, Redis \(cluster mode disabled\), or Redis \(cluster mode enabled\)](SelectEngine.Uses.md)
+ [Determine Available Engine Versions](SelectEngine.RegionVersions.md)
+ [ElastiCache for Memcached Versions](SelectEngine.MemcachedVersions.md)
+ [ElastiCache for Redis Versions](SelectEngine.RedisVersions.md)
+ [Upgrading Engine Versions](VersionManagement.md)
+ [Maintenance Window](VersionManagement.MaintenanceWindow.md)