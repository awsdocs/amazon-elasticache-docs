# Adding Auto Discovery To Your Client Library<a name="AutoDiscovery.AddingToYourClientLibrary"></a>

The configuration information for Auto Discovery is stored redundantly in each cache cluster node\. Client applications can query any cache node and obtain the configuration information for all of the nodes in the cluster\.

The way in which an application does this depends upon the cache engine version:
+ If the cache engine version is **1\.4\.14 or higher**, use the `config` command\.
+ If the cache engine version is **lower than 1\.4\.14**, use the `get AmazonElastiCache:cluster` command\.

The outputs from these two commands are identical, and are described in the [Output Format](#AutoDiscovery.AddingToYourClientLibrary.OutputFormat) section below\.

## Cache Engine Version 1\.4\.14 or Higher<a name="AutoDiscovery.AddingToYourClientLibrary.1-4-14-plus"></a>

For cache engine version 1\.4\.14 or higher, use the `config` command\. This command has been added to the Memcached ASCII and binary protocols by ElastiCache, and is implemented in the ElastiCache Cluster Client\. If you want to use Auto Discovery with another client library, then that library will need to be extended to support the `config` command\.

**Note**  
The following documentation pertains to the ASCII protocol; however, the `config` command supports both ASCII and binary\. If you want to add Auto Discovery support using the binary protocol, refer to the [source code for the ElastiCache Cluster Client](https://github.com/amazonwebservices/aws-elasticache-cluster-client-memcached-for-java/tree/master/src/main/java/net/spy/memcached/protocol/binary)\.

**Syntax**

`config [sub-command] [key]`

### Options<a name="AutoDiscovery.AddingToYourClientLibrary.1-4-14-plus.Options"></a>


| Name | Description | Required | 
| --- | --- | --- | 
| sub\-command |  The sub\-command used to interact with a cache node\. For Auto Discovery, this sub\-command is `get`\.  | Yes | 
| key |  The key under which the cluster configuration is stored\. For Auto Discovery, this key is named `cluster`\.  | Yes | 

To get the cluster configuration information, use the following command: 

```
config get cluster
```

## Cache Engine Version Lower Than 1\.4\.14<a name="AutoDiscovery.AddingToYourClientLibrary.pre-1-4-14"></a>

To get the cluster configuration information, use the following command: 

```
get AmazonElastiCache:cluster
```

**Note**  
Do not tamper with the "AmazonElastiCache:cluster" key, since this is where the cluster configuration information resides\. If you do overwrite this key, then the client may be incorrectly configured for a brief period of time \(no more than 15 seconds\) before ElastiCache automatically and correctly updates the configuration information\.

## Output Format<a name="AutoDiscovery.AddingToYourClientLibrary.OutputFormat"></a>

Whether you use `config get cluster` or `get AmazonElastiCache:cluster`, the reply consists of two lines:
+ The version number of the configuration information\. Each time a node is added or removed from the cache cluster, the version number increases by one\. 
+ A list of cache nodes\. Each node in the list is represented by a *hostname\|ip\-address\|port* group, and each node is delimited by a space\. 

A carriage return and a linefeed character \(CR \+ LF\) appears at the end of each line\. The data line contains a linefeed character \(LF\) at the end, to which the CR \+ LF is added\. The config version line is terminated by LF without the CR\. 

A cache cluster containing three nodes would be represented as follows:

```
configversion\n
hostname|ip-address|port hostname|ip-address|port hostname|ip-address|port\n\r\n
```

Each node is shown with both the CNAME and the private IP address\. The CNAME will always be present; if the private IP address is not available, it will not be shown; however, the pipe characters "`|`" will still be printed\.

**Example**  
Here is an example of the payload returned when you query the configuration information:  

```
CONFIG cluster 0 136\r\n
12\n
myCluster.pc4ldq.0001.use1.cache.amazonaws.com|10.82.235.120|11211 myCluster.pc4ldq.0002.use1.cache.amazonaws.com|10.80.249.27|11211\n\r\n 
END\r\n
```

**Note**  
The second line indicates that the configuration information has been modified twelve times so far\.
In the third line, the list of nodes is in alphabetical order by hostname\. This ordering might be in a different sequence from what you are currently using in your client application\.