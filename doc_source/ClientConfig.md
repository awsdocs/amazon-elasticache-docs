# Configuring ElastiCache Clients<a name="ClientConfig"></a>

An ElastiCache cluster is protocol\-compliant with Memcached or Redis, depending on which cache engine was chosen when the cluster was created\. The code, applications, and most popular tools that you use today with your existing Memcached or Redis environments will work seamlessly with the service\.

This section discusses specific considerations for connecting to cache nodes in ElastiCache\.


+ [Restricted Commands](ClientConfig.RestrictedCommands.md)
+ [Finding Node Endpoints and Port Numbers](ClientConfig.FindingEndpointsAndPorts.md)
+ [Connecting for Using Auto Discovery](ClientConfig.AutoDiscovery.md)
+ [Connecting to Nodes in a Redis Cluster](ClientConfig.ReplicationGroup.md)
+ [DNS Names and Underlying IP](ClientConfig.DNS.md)