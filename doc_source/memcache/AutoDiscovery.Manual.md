# Connecting to Cache Nodes Manually<a name="AutoDiscovery.Manual"></a>

If your client program does not use Auto Discovery, it can manually connect to each of the cache nodes\. This is the default behavior for Memcached clients\.

You can obtain a list of cache node hostnames and port numbers from the [AWS Management Console](http://aws.amazon.com/console/)\. You can also use the AWS CLI `aws elasticache describe-cache-clusters` command with the `--show-cache-node-info` parameter\.

**Example**  
The following Java code snippet shows how to connect to all of the nodes in a four\-node cache cluster:  

```
...

ArrayList<String> cacheNodes = new ArrayList<String>(
	Arrays.asList(
	    "mycachecluster.fnjyzo.0001.use1.cache.amazonaws.com:11211",
	    "mycachecluster.fnjyzo.0002.use1.cache.amazonaws.com:11211",
	    "mycachecluster.fnjyzo.0003.use1.cache.amazonaws.com:11211",
	    "mycachecluster.fnjyzo.0004.use1.cache.amazonaws.com:11211"));
	      
MemcachedClient cache = new MemcachedClient(AddrUtil.getAddresses(cacheNodes));

...
```

**Important**  
If you scale up or scale down your cache cluster by adding or removing nodes, you will need to update the list of nodes in the client code\.