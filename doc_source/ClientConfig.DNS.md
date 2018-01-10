# DNS Names and Underlying IP<a name="ClientConfig.DNS"></a>

Memcached and Redis clients maintain a server list containing the addresses and ports of the servers holding the cache data\. When using ElastiCache, the DescribeCacheClusters API \(or the describe\-cache\-clusters command line utility\) returns a fully qualified DNS entry and port number that can be used for the server list\.

**Important**  
It is important that client applications are configured to frequently resolve DNS names of cache nodes when they attempt to connect to a cache node endpoint\.

**VPC Installations**

ElastiCache ensures that both the DNS name and the IP address of the cache node remain the same when cache nodes are recovered in case of failure\.

**Non\-VPC Installations**

ElastiCache ensures that the DNS name of a cache node is unchanged when cache nodes are recovered in case of failure; however, the underlying IP address of the cache node can change\.

Most Memcached and Redis client libraries support persistent cache node connections by default\. We recommend using persistent cache node connections when using ElastiCache\. Client\-side DNS caching can occur in multiple places, including client libraries, the language runtime, or the client operating system\. You should review your application configuration at each layer to ensure that you are frequently resolving IP addresses for your cache nodes\.