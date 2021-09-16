# Troubleshooting<a name="TroubleshootingConnections"></a>

The following items must be verified while troubleshooting persistent connectivity issues with ElastiCache:

**Topics**
+ [Security groups](#Security_groups)
+ [Network ACLs](#Network_ACLs)
+ [Route tables](#Route_tables)
+ [DNS resolution](#DNS_Resolution)
+ [Identifying issues with server\-side diagnostics](#Diagnostics)
+ [Network connectivity validation](#Connectivity)
+ [Network\-related limits](#Network-limits)
+ [CPU Usage](#CPU-Usage)
+ [Connections being terminated from the server side](#Connections-server)
+ [Client\-side troubleshooting for Amazon EC2 instances](#Connections-client)
+ [Dissecting the time taken to complete a single request](#Dissecting-time)

## Security groups<a name="Security_groups"></a>

Security Groups are virtual firewalls protecting your ElastiCache client \(EC2 instance, AWS Lambda function, Amazon ECS container, etc\.\) and ElastiCache cluster\. Security groups are stateful, meaning that after the incoming or outgoing traffic is allowed, the responses for that traffic will be automatically authorized in the context of that specific security group\.

 The stateful feature requires the security group to keep track of all authorized connections, and there is a limit for tracked connections\. If the limit is reached, new connections will fail\. Please refer to the troubleshooting section for help on how to identify if the limits has been hit on the client or Elasticache side\.

You can have a single security groups assigned at the same time to the client and ElastiCache cluster, or individual security groups for each\.

For both cases, you need to allow the TCP outbound traffic on the ElastiCache port from the source and the inbound traffic on the same port to ElastiCache\. The default port is 11211 for Memcached and 6379 for Redis\. By default, security groups allow all outbound traffic\. In this case, only the inbound rule in the target security group is required\.

For more information, see [Access patterns for accessing an ElastiCache cluster in an Amazon VPC](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-vpc-accessing.html)\.

## Network ACLs<a name="Network_ACLs"></a>

Network Access Control Lists \(ACLs\) are stateless rules\. The traffic must be allowed in both directions \(Inbound and Outbound\) to succeed\. Network ACLs are assigned to subnets, not specific resources\. It is possible to have the same ACL assigned to ElastiCache and the client resource, especially if they are in the same subnet\.

By default, network ACLs allow all trafic\. However, it is possible to customize them to deny or allow traffic\. Additionally, the evaluation of ACL rules is sequential, meaning that the rule with the lowest number matching the traffic will allow or deny it\. The minimum configuration to allow the Redis traffic is:

Client side Network ACL:
+ **Inbound Rules:**
+ Rule number: preferably lower than any deny rule;
+ Type: Custom TCP Rule;
+ Protocol: TCP
+ Port Range: 1024\-65535
+ Source: 0\.0\.0\.0/0 \(or create individial rules for the ElastiCache cluster subnets\)
+ Allow/Deny: Allow
+ **Outbound Rules:**
+ Rule number: preferably lower than any deny rule;
+ Type: Custom TCP Rule;
+ Protocol: TCP
+ Port Range: 6379
+ Source: 0\.0\.0\.0/0 \(or the ElastiCache cluster subnets\. Keep in mind that using specific IPs may create issues in case of failover or scaling the cluster\)
+ Allow/Deny: Allow

ElastiCache Network ACL:
+ **Inbound Rules:**
+ Rule number: preferably lower than any deny rule;
+ Type: Custom TCP Rule;
+ Protocol: TCP
+ Port Range: 6379
+ Source: 0\.0\.0\.0/0 \(or create individial rules for the ElastiCache cluster subnets\)
+ Allow/Deny: Allow
+ **Outbound Rules:**
+ Rule number: preferably lower than any deny rule;
+ Type: Custom TCP Rule;
+ Protocol: TCP
+ Port Range: 1024\-65535
+ Source: 0\.0\.0\.0/0 \(or the ElastiCache cluster subnets\. Keep in mind that using specific IPs may create issues in case of failover or scaling the cluster\)
+ Allow/Deny: Allow

For more information, see [Network ACLs](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-network-acls.html)\.

## Route tables<a name="Route_tables"></a>

Similarly to Network ACLs, each subnet can have different route tables\. If clients and the ElastiCache cluster are in different subnets, make sure that their route tables allow them to reach each other\.

More complex environments, involving multiple VPCs, dynamic routing, or network firewalls, may become difficult to troubleshoot\. See [Network connectivity validation](#Connectivity) to confirm that your network settings are appropriate\.

## DNS resolution<a name="DNS_Resolution"></a>

ElastiCache provides the service endpoints based on DNS names\. The endpoints available are `Configuration`, `Primary`, `Reader`, and `Node` endpoints\. For more information, see [Finding Connection Endpoints](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Endpoints.html)\.

In case of failover or cluster modification, the address associated to the endpoint name may change and will be automatically updated\.

Custom DNS settings \(i\.e\., not using the VPC DNS service\) may not be aware of the ElastiCache\-provided DNS names\. Make sure that your system can successfully resolve the ElastiCache endpoints using system tools like `dig` \(as shown following\) or `nslookup`\.

```
$ dig +short example.xxxxxx.ng.0001.use1.cache.amazonaws.com
example-001.xxxxxx.0001.use1.cache.amazonaws.com.
1.2.3.4
```

You can also force the name resolution through the VPC DNS service:

```
$ dig +short example.xxxxxx.ng.0001.use1.cache.amazonaws.com @169.254.169.253
example-001.tihewd.0001.use1.cache.amazonaws.com.
1.2.3.4
```

## Identifying issues with server\-side diagnostics<a name="Diagnostics"></a>

CloudWatch metrics and run\-time information from the ElastiCache engine are common sources or information to identify potential sources of connection issues\. A good analysis commonly starts with the following items:
+ CPU usage: Redis is a multi\-threaded application\. However, execution of each command happens in a single \(main\) thread\. For this reason, ElastiCache provides the metrics `CPUUtilization` and `EngineCPUUtilization`\. `EngineCPUUtilization` provides the CPU utilization dedicated to the Redis process, and `CPUUtilization` the usage across all vCPUs\. Nodes with more than one vCPU usually have different values for `CPUUtilization` and `EngineCPUUtilization`, the second being commonly higher\. High `EngineCPUUtilization` can be caused by an elevated number of requests or complex operations that take a significant amount of CPU time to complete\. You can identify both with the following:
  + Elevated number of requests: Check for increases on other metrics matching the `EngineCPUUtilization` pattern\. Useful metrics are:
    + `CacheHits` and `CacheMisses`: the number of successful requests or requests that didn’t find a valid item in the cache\. If the ratio of misses compared to hits is high, the application is wasting time and resources with unfruitful requests\.
    + `SetTypeCmds` and `GetTypeCmds`: These metrics correlated with `EngineCPUUtilization` can help to understand if the load is significantly higher for write requests, measured by `SetTypeCmds`, or reads, measured by `GetTypeCmds`\. If the load is predominantly reads, using multiple read\-replicas can balance the requests across multiple nodes and spare the primary for writes\. In cluster mode\-disabled clusters, the use of read\-replicas can be done by creating an additional connection configuration in the application using the ElastiCache reader endpoint\. For more information, see [Finding Connection Endpoints](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Endpoints.html)\. The read operations must be submitted to this additional connection\. Write operations will be done through the regular primary endpoint\. In cluster mode\-enabled, it is advisable to use a library that supports read replicas natively\. With the right flags, the library will be able to automatically discover the cluster topology, the replica nodes, enable the read operations through the [READONLY](https://redis.io/commands/readonly) Redis command, and submit the read requests to the replicas\.
  + Elevated number of connections:
    + `CurrConnections` and `NewConnections`: `CurrConnection` is the number of established connections at the moment of the datapoint collection, while `NewConnections` shows how many connections were created in the period\.

      Creating and handling connections implies significant CPU overhead\. Additionally, the TCP three\-way handshake required to create new connections will negatively affect the overall response times\.

      An ElastiCache node with thousands of `NewConnections` per minute indicates that a connection is created and used by just a few commands, which is not optimal\. Keeping connections established and reusing them for new operations is a best practice\. This is possible when the client application supports and properly implements connection pooling or persistent connections\. With connection pooling, the number of `currConnections` does not have big variations, and the `NewConnections` should be as low as possible\. Redis provides optimal performance with small number of currConnections\. Keeping currConnection in the order of tens or hundreds minimizes the usage of resources to support individual connections like client buffers and CPU cycles to serve the connection\.
  + Network throughput:
    + Determine the bandwidth: ElastiCache nodes have network bandwidth proportional to the node size\. Since applications have different characteristics, the results can vary according to the workload\. As examples, applications with high rate of small requests tend to affect more the CPU usage than the network throughput while bigger keys will cause higher network utilization\. For that reason, it is advisable to test the nodes with the actual workload for a better understanding of the limits\.

      Simulating the load from the application would provide more accurate results\. However, benchmark tools can give a good idea of the limits\.
    + For cases where the requests are predominantly reads, using replicas for read operations will alleviate the load on the primary node\. If the use\-case is predominantly writes, the use of many replicas will amplify the network usage\. For every byte written to the primary node, N bytes will be sent to the replicas,being N the number of replicas\. The best practice for write intensive workloads are using ElastiCache for Redis with cluster mode\-enabled so the writes can be balanced across multiple shards, or scale\-up to a node type with more network capabilities\.
    + The CloudWatchmetrics `NetworkBytesIn` and `NetworkBytesOut` provide the amount of data coming into or leaving the node, respectively\. `ReplicationBytes` is the traffic dedicated to data replication\.

    For more information, see [Network\-related limits](#Network-limits)\.
  + Complex commands: Redis commands are served on a single thread, meaning that requests are served sequentially\. A single slow command can affect other requests and connections, culminating in time\-outs\. The use of commands that act upon multiple values, keys, or data types must be done carefully\. Connections can be blocked or terminated depending on the number of parameters, or size of its input or output values\.

    A notorious example is the `KEYS` command\. It sweeps the entire keyspace searching for a given pattern and blocks the execution of other commands during its execution\. Redis uses the “Big O” notation to describe its commands complexity\.

    Keys command has O\(N\) time complexity, N being the number of keys in the database\. Therefore, the larger the number of keys, the slower the command will be\. `KEYS` can cause trouble in different ways: If no search pattern is used, the command will return all key names available\. In databases with thousand or million of items, a huge output will be created and flood the network buffers\.

    If a search pattern is used, only the keys matching the pattern will return to the client\. However, the engine still sweeps the entire keyspace searching for it, and the time to complete the command will be the same\. 

    An alternative for `KEYS` is the `SCAN` command\. It iterates over the keyspace and limits the iterations in a specific number of items, avoiding prolonged blocks on the engine\.

    Scan has the `COUNT` parameter, used to set the size of the iteration blocks\. The default value is 10 \(10 items per iteration\)\.

    Depending on the number of items in the database, small `COUNT` values blocks will require more iterations to complete a full scan, and bigger values will keep the engine busy for longer at each iteration\. While small count values will make `SCAN` slower on big databases, larger values can cause the same issues mentioned for `KEYS`\.

    As an example, running the `SCAN` command with count value as 10 will requires 100,000 repetitions on a database with 1 million keys\. If the average network round\-trip time is 0\.5 milliseconds, approximately 50,000 milliseconds \(50 seconds\) will be spent transferring requests\.

    On the other hand, if the count value were 100,0000, a single iteration would be required and only 0\.5 ms would be spent transferring it\. However, the engine would be entirely blocked for other operations until the command finishes sweeping all the keyspace\. 

    Besides `KEYS`, several other commands are potentially harmful if not used correctly\. To see a list of all commands and their respective time complexity, go to [https://redis\.io/commands](https://redis.io/commands)\.

    Examples of potential issues:
    + Lua scripts: Redis provides an embedded Lua interpreter, allowing the execution of scripts on the server\-side\. Lua scripts on Redis are executed on engine level and are atomic by definition, meaning that no other command or script will be allowed to run while a script is in execution\. Lua scripts provide the possibility of running multiple commands, decision\-making algorithms, data parsing, and others directly on the Redis engine\. While the atomicity of scripts and the chance of offloading the application are tempting, scripts must be used with care and for small operations\. On ElastiCache, the execution time of Lua scripts is limited to 5 seconds\. Scripts that haven’t written to the keyspace will be automatically terminated after the 5 seconds period\. To avoid data corruption and inconsistencies, the node will failover if the script execution hasn’t completed in 5 seconds and had any write during its execution\. [Transactions](https://redis.io/topics/transactions) are the alternative to guarantee consistency of multiple related key modifications in Redis\. A transaction allows the execution of a block of commands, watching existing keys for modifications\. If any of the watched keys changes before the completion of the transaction, all modifications are discarded\.
    + Mass deletion of items: The `DEL` command accepts multiple parameters, which are the key names to be deleted\. Deletion operations are synchronous and will take significant CPU time if the list of parameters is big, or contains a big list, set, sorted set, or hash \(data structures holding several sub\-items\)\. In other words, even the deletion of a single key can take significant time if it has many elements\. The alternative to `DEL` is `UNLINK`, which is an asynchronous command available since Redis 4\. `UNLINK` must be preferred over `DEL` whenever possible\. Starting on ElastiCache for Redis 6x, the `lazyfree-lazy-user-del` parameter makes the `DEL` command behave like `UNLINK` when enabled\. For more information, see [Redis 6\.x Parameter Changes](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.Redis.html#ParameterGroups.Redis.6-0-x)\. 
    + Commands acting upon multiple keys: `DEL` was mentioned before as a command that accepts multiple arguments and its execution time will be directly proportional to that\. However, Redis provides many more commands that work similarly\. As examples, `MSET` and `MGET` allow the insertion or retrieval of multiple String keys at once\. Their usage may be beneficial to reduce the network latency inherent to multiple individual `SET` or `GET` commands\. However, an extensive list of parameters will affect CPU usage\.

       While CPU utilization alone is not the cause for connectivity issues, spending too much time to process a single or few commands over multiple keys may cause failure of other requests and increase overall CPU utilization\.

       The number of keys and their size will affect the command complexity and consequently completion time\.

       Other examples of commands that can act upon multiple keys: `HMGET`, `HMSET`, `MSETNX`, `PFCOUNT`, `PFMERGE`, `SDIFF`, `SDIFFSTORE`, `SINTER`, `SINTERSTORE`, `SUNION`, `SUNIONSTORE`, `TOUCH`, `ZDIFF`, `ZDIFFSTORE`, `ZINTER` or `ZINTERSTORE`\.
    + Commands acting upon multiple data types: Redis also provides commands that act upon one or multiple keys, regardless of their data type\. ElastiCache for Redis provides the metric `KeyBasedCmds` to monitor such commands\. This metric sums the execution of the following commands in the selected period:
      + O\(N\) complexity:
        + `KEYS`
      + O\(1\)
        + `EXISTS`
        + `OBJECT`
        + `PTTL`
        + `RANDOMKEY`
        + `TTL`
        + `TYPE`
        + `EXPIRE`
        + `EXPIREAT`
        + `MOVE`
        + `PERSIST`
        + `PEXPIRE`
        + `PEXPIREAT`
        + `UNLINK (O(N)` to reclaim memory\. However the memory\-reclaiming task happens in a separated thread and does not block the engine
      + Different complexity times depending on the data type:
        + `DEL`
        + `DUMP`
        + `RENAME` is considered a command with O\(1\) complexity, but executes `DEL` internally\. The execution time will vary depending on the size of the renamed key\.
        + `RENAMENX`
        + `RESTORE`
        + `SORT`
      + Big hashes: Hash is a data type that allows a single key with multiple key\-value sub\-items\. Each hash can store 4\.294\.967\.295 items and operations on big hashes can become expensive\. Similarly to `KEYS`, hashes have the `HKEYS` command with O\(N\) time complexity, N being the number of items in the hash\. `HSCAN` must be preferred over `HKEYS` to avoid long running commands\. `HDEL`, `HGETALL`, `HMGET`, `HMSET` and `HVALS` are commands that should be used with caution on big hashes\.
    + Other big data\-structures: Besides hashes, other data structures can be CPU intensive\. Sets, Lists, Sorted Sets, and Hyperloglogs can also take significant time to be manipulated depending on their size and commands used\. For more information on those commands, see [https://redis\.io/commands](https://redis.io/commands)\.

## Network connectivity validation<a name="Connectivity"></a>

After reviewing the network configurations related to DNS resolution, security groups, network ACLs, and route tables, the connectivity can be validated with the VPC Reachability Analyzer and system tools\.

Reachability Analyzer will test the network connectivity and confirm if all the requirements and permissions are satisfied\. For the tests below you will need the ENI ID \(Elastic Network Interface Identification\) of one of the ElastiCache nodes available in your VPC\. You can find it by doing the following:

1. Go to [https://console\.aws\.amazon\.com/ec2/v2/home?\#NIC:](https://console.aws.amazon.com/ec2/v2/home?#NIC)

1. Filter the interface list by your Elasticache cluster name or the IP address got from the DNS validations previously\.

1. Write down or otherwise save the ENI ID\. If multiple interfaces are shown, review the description to confirm that they belong to the right ElastiCache cluster and choose one of them\.

1. Proceed to the next step\.

1. Create an analyze path at [https://console\.aws\.amazon\.com/vpc/home?\#ReachabilityAnalyzer](https://console.aws.amazon.com/vpc/home?#ReachabilityAnalyzer) and choose the following options:
   + **Source Type**: Choose **instance** if your ElastiCache client runs on an Amazon EC2 instance or **Network Interface** if it uses another service, such as AWS Fargate Amazon ECS with awsvpc network, AWS Lambda, etc\), and the respective resource ID \(EC2 instance or ENI ID\);
   + **Destination Type**: Choose **Network Interface** and select the **Elasticache ENI** from the list\.
   + **Destination port**: specify 6379 for ElastiCache for Redis or 11211 for ElastiCache for Memcached\. Those are the ports defined with the default configuration and this example assumes that they are not changed\.
   + **Protocol**: TCP

Create the analyze path and wait a few moments for the result\. If the status is unreachable, open the analysis details and review the **Analysis explorer** for details where the requests were blocked\.

If the reachability tests passed, proceed to the verification on the OS level\.

To validate the TCP connectivity on the ElastiCache service port: On Amazon Linux, `Nping` is available in the package `nmap` and can test the TCP connectivity on the ElastiCache port, as well as providing the network round\-trip time to establish the connection\. Use this to validate the network connectivity and the current latency to the ElastiCache cluster, as shown following: 

```
$ sudo nping --tcp -p 6379 example.xxxxxx.ng.0001.use1.cache.amazonaws.com

Starting Nping 0.6.40 ( http://nmap.org/nping ) at 2020-12-30 16:48 UTC
SENT (0.0495s) TCP ...
(Output suppressed )

Max rtt: 0.937ms | Min rtt: 0.318ms | Avg rtt: 0.449ms
Raw packets sent: 5 (200B) | Rcvd: 5 (220B) | Lost: 0 (0.00%)
Nping done: 1 IP address pinged in 4.08 seconds
```

By default, `nping` sends 5 probes with a delay of 1 second between them\. You can use the option “\-c” to increase the number of probes and “\-\-delay“ to change the time to send a new test\. 

If the tests with `nping` fail and the *VPC Reachability Analyzer* tests passed, ask your system administrator to review possible Host\-based firewall rules, asymmetric routing rules, or any other possible restriction on the operating system level\.

On the ElastiCache console, check if **Encryption in\-transit** is enabled in your ElastiCache cluster details\. If in\-transit encryption is enabled, confirm if the TLS session can be established with the following command:

```
openssl s_client -connect example.xxxxxx.use1.cache.amazonaws.com:6379
```

An extensive output is expected if the connection and TLS negotiation are successful\. Check the return code available in the last line, the value must be `0 (ok)`\. If openssl returns something different, check the reason for the error at [https://www\.openssl\.org/docs/man1\.0\.2/man1/verify\.html\#DIAGNOSTICS](https://www.openssl.org/docs/man1.0.2/man1/verify.html#DIAGNOSTICS)\.

If all the infrastructure and operating system tests passed but your application is still unable to connect to ElastiCache, check if the application configurations are compliant with the ElastiCache settings\. Common mistakes are:
+ Your application does not support ElastiCache cluster mode, and ElastiCache has cluster\-mode enabled;
+ Your application does not support TLS/SSL, and ElastiCache has in\-transit encryption enabled; 
+ Application supports TLS/SSL but does not have the right configuration flags or trusted certification authorities; 

## Network\-related limits<a name="Network-limits"></a>
+ Maximum number of connections: There are hard limits for simultaneous connections\. Each ElastiCache node allows up to 65,000 simultaneous connections across all clients\. This limit can be monitored through the `CurrConnections` metrics on CloudWatch\. However, clients also have their limits for outbound connections\.On Linux, check the allowed ephemeral port range with the command:

  ```
  # sysctl net.ipv4.ip_local_port_range
  net.ipv4.ip_local_port_range = 32768 60999
  ```

  In the previous example, 28231 connections will be allowed from the same source, to the same destination IP \(ElastiCache node\) and port\. The following command shows how many connections exist for a specific Elasticache node \(IP 1\.2\.3\.4\):

  ```
  ss --numeric --tcp state connected "dst 1.2.3.4 and dport == 6379" | grep -vE '^State' | wc -l
  ```

  If the number is too high, your system may become overloaded trying to process the connection requests\. It is advisable to consider implementing techniques like connection pooling or persistent connections to better handle the connections\. Whenever possible, configure the connection pool to limit the maximum number of connections to a few hundred\. Also, back\-off logic to handle time\-outs or other connection exceptions would are advisable to avoid connection churn in case of issues\.
+ Network traffic limits: Check the following [CloudWatch metrics for Redis](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/CacheMetrics.Redis.html) to identify possible network limits being hit on the ElastiCache node:
  + `NetworkBandwidthInAllowanceExceeded` / `NetworkBandwidthOutAllowanceExceeded`: Network packets shaped because the throughput exceeded the aggregated bandwidth limit\.

    It is important to note that every byte written to the primary node will be replicated to N replicas, N being the number of replicas\. Clusters with small node types, multiple replicas, and intensive write requests may not be able to cope with the replication backlog\. For such cases, it's a best practice to scale\-up \(change node type\), scale\-out \(add shards in cluster\-mode enabled clusters\), reduce the number of replicas, or minimize the number of writes\.
  + `NetworkConntrackAllowanceExceeded`: Packets shaped because the maximum number of connections tracked across all security groups assigned to the node has been exceeded\. New connections will likely fail during this period\.
  + `NetworkPackets PerSecondAllowanceExceeded`: Maximum number of packets per second exceeded\. Workloads based on a high rate of very small requests may hit this limit before the maximum bandwidth\.

  The metrics above are the ideal way to confirm nodes hitting their network limits\. However, limits are also identifiable by plateaus on network metrics\.

  If the plateaus are observed for extended periods, they will be likely followed by replication lag, increase on bytes Used for cache, drop on freeable memory, high swap and CPU usage\. Amazon EC2 instances also have network limits that can tracked through [ENA driver metrics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-network-performance-ena.html)\. Linux instances with enhanced networking support and ENA drivers 2\.2\.10 or newer can review the limit counters with the command:

  ```
  # ethtool -S eth0 | grep "allowance_exceeded"
  ```

## CPU Usage<a name="CPU-Usage"></a>

The CPU usage metric is the starting point of investigation, and the following items can help to narrow down possible issues on the ElastiCache side:
+ Redis SlowLogs: The ElastiCache default configuration retains the last 128 commands that took over 10 milliseconds to complete\. The history of slow commands is kept during the engine runtime and will be lost in case of failure or restart\. If the list reaches 128 entries, old events will be removed to open room for new ones\. The size of the list of slow events and the execution time considered slow can by modified via the parameters `slowlog-max-len` and `slowlog-log-slower-than` in a [custom parameter group](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/ParameterGroups.html)\. The slowlogs list can be retrieved by running `SLOWLOG GET 128` on the engine, 128 being the last 128 slow commands reported\. Each entry has the following fields:

  ```
  1) 1) (integer) 1 -----------> Sequential ID
     2) (integer) 1609010767 --> Timestamp (Unix epoch time)of the Event
     3) (integer) 4823378 -----> Time in microseconds to complete the command.
     4) 1) "keys" -------------> Command
        2) "*" ----------------> Arguments 
     5) "1.2.3.4:57004"-> Source
  ```

  The event above happened on December 26, at 19:26:07 UTC, took 4\.8 seconds \(4\.823ms\) to complete and was caused by the `KEYS` command requested from the client 1\.2\.3\.4\.

  On Linux, the timestamp can be converted with the command date:

  ```
  $ date --date='@1609010767'
  Sat Dec 26 19:26:07 UTC 2020
  ```

  With Python:

  ```
  >>> from datetime import datetime
  >>> datetime.fromtimestamp(1609010767)
  datetime.datetime(2020, 12, 26, 19, 26, 7)
  ```

  Or on Windows with PowerShell:

  ```
  PS D:\Users\user> [datetimeoffset]::FromUnixTimeSeconds('1609010767')
  DateTime      : 12/26/2020 7:26:07 PM
  UtcDateTime  
                  : 12/26/2020 7:26:07 PM
  LocalDateTime : 12/26/2020 2:26:07 PM
  Date          : 12/26/2020 12:00:00 AM
  Day           : 26
  DayOfWeek    
                  : Saturday
  DayOfYear     : 361
  Hour          : 19
  Millisecond   : 0
  Minute        : 26
  Month        
                  : 12
  Offset        : 00:00:00Ticks         : 637446075670000000
  UtcTicks     
                  : 637446075670000000
  TimeOfDay     : 19:26:07
  Year          : 2020
  ```

  Many slow commands in a short period of time \(same minute or less\) is a reason for concern\. Review the nature of commands and how they can be optimized \(see previous examples\)\. If commands with O\(1\) time complexity are frequently reported, check the other factors for high CPU usage mentioned before\.
+ Latency metrics: ElastiCache for Redis provides CloudWatch metrics to monitor the average latency for different classes of commands\. The datapoint is calculated by dividing the total number of executions of commands in the category by the total execution time in the period\. It is important to understand that latency metric results are an aggregate of multiple commands\. A single command can cause unexpected results, like timeouts, without significant impact on the metrics\. For such cases, the slowlog events would be a more accurate source of information\. The following list contains the latency metrics available and the respective commands that affect them\.
  + EvalBasedCmdsLatency: related to Lua Script commands, `eval`, `evalsha`;
  + GeoSpatialBasedCmdsLatency: `geodist`, `geohash`, `geopos`, `georadius`, `georadiusbymember`, `geoadd`;
  + GetTypeCmdsLatency: Read commands, regardless of data type;
  + HashBasedCmdsLatency: `hexists`, `hget`, `hgetall`, `hkeys`, `hlen`, `hmget`, `hvals`, `hstrlen`, `hdel`, `hincrby`, `hincrbyfloat`, `hmset`, `hset`, `hsetnx`;
  + HyperLogLogBasedCmdsLatency: `pfselftest`, `pfcount`, `pfdebug`, `pfadd`, `pfmerge`;
  + KeyBasedCmdsLatency: Commands that can act upon different data types: `dump`, `exists`, `keys`, `object`, `pttl`, `randomkey`, `ttl`, `type`, `del`, `expire`, `expireat`, `move`, `persist`, `pexpire`, `pexpireat`, `rename`, `renamenx`, `restoreK`, `sort`, `unlink`;
  + ListBasedCmdsLatency: lindex, llen, lrange, blpop, brpop, brpoplpush, linsert, lpop, lpush, lpushx, lrem, lset, ltrim, rpop, rpoplpush, rpush, rpushx; 
  + PubSubBasedCmdsLatency: psubscribe, publish, pubsub, punsubscribe, subscribe, unsubscribe; 
  + SetBasedCmdsLatency: `scard`, `sdiff`, `sinter`, `sismember`, `smembers`, `srandmember`, `sunion`, `sadd`, `sdiffstore`, `sinterstore`, `smove`, `spop`, `srem`, `sunionstore`; 
  + SetTypeCmdsLatency: Write commands, regardless of data\-type;
  + SortedSetBasedCmdsLatency: `zcard`, `zcount`, `zrange`, `zrangebyscore`, `zrank`, `zrevrange`, `zrevrangebyscore`, `zrevrank`, `zscore`, `zrangebylex`, `zrevrangebylex`, `zlexcount`, `zadd`\. `zincrby`, `zinterstore`, `zrem`, `zremrangebyrank`, `zremrangebyscore`, `zunionstore`, `zremrangebylex`, `zpopmax`, `zpopmin`, `bzpopmin`, `bzpopmax`; 
  + StringBasedCmdsLatency: `bitcount`, `get`, `getbit`, `getrange`, `mget`, `strlen`, `substr`, `bitpos`, `append`, `bitop`, `bitfield`, `decr`, `decrby`, `getset`, `incr`, `incrby`, `incrbyfloat`, `mset`, `msetnx`, `psetex`, `set`, `setbit`, `setex`, `setnx`, `setrange`; 
  + StreamBasedCmdsLatency: `xrange`, `xrevrange`, `xlen`, `xread`, `xpending`, `xinfo`, `xadd`, `xgroup`, `readgroup`, `xack`, `xclaim`, `xdel`, `xtrim`, `xsetid`; 
+ Redis runtime commands: 
  + info commandstats: Provides a list of commands executed since the Redis engine started, their cumulative executions number, total execution time, and average execution time per command;
  + client list: Provides a list of currently connected clients and relevant information like buffers usage, last command executed, etc;
+ Backup and replication: ElastiCache for Redis versions earlier than 2\.8\.22 use a forked process to create backups and process full syncs with the replicas\. This method may incur in significant memory overhead for write intensive use\-cases\.

  Starting with Elasticache Redis 2\.8\.22, AWS introduced a forkless backup and replication method\. The new method may delay writes in order to prevent failures\. Both methods can cause periods of higher CPU utilization, lead to higher response times and consequently lead to client timeouts during their execution\. Always check if the client failures happen during the backup window or the `SaveInProgress` metric was 1 in the period\. It is advisable to schedule the backup window for periods of low utilization to minimize the possibility of issues with clients or backup failures\.

## Connections being terminated from the server side<a name="Connections-server"></a>

The default ElastiCache for Redis configuration keeps the client connections established indefinitely\. However, in some cases connection termination may be desirable\. For example:
+ Bugs in the client application may cause connections to be forgotten and kept established with an idle state\. This is called "connection leak“ and the consequence is a steady increase on the number of established connections observed on the `CurrConnections` metric\. This behavior can result in saturation on the client or ElastiCache side\. When an immediate fix is not possible from the client side, some administrators set a ”timeout“ value in their ElastiCache parameter group\. The timeout is the time in seconds allowed for idle connections to persist\. If the client doesn’t submit any request in the period, the Redis engine will terminate the connection as soon as the connection reaches the timeout value\. Small timeout values may result in unnecessary disconnections and clients will need handle them properly and reconnect, causing delays\.
+ The memory used to store keys is shared with client buffers\. Slow clients with big requests or responses may demand a significant amount of memory to handle its buffers\. The default ElastiCache for Redis configurations does not restrict the size of regular client output buffers\. If the `maxmemory` limit is hit, the engine will try to evict items to fulfill the buffer usage\. In extreme low memory conditions, ElastiCache for Redis might choose to disconnect clients that consume large client output buffers in order to free memory and retain the cluster’s health\. 

  It is possible to limit the size of client buffers with custom configurations and clients hitting the limit will be disconnected\. However, clients should be able to handle unexpected disconnections\. The parameters to handle buffers size for regular clients are the following:
  + client\-query\-buffer\-limit: Maximum size of a single input request;
  + client\-output\-buffer\-limit\-normal\-soft\-limit: Soft limit for client connections\. The connection will be terminated if stays above the soft limit for more than the time in seconds defined on client\-output\-buffer\-limit\-normal\-soft\-seconds or if it hits the hard limit;
  + client\-output\-buffer\-limit\-normal\-soft\-seconds: Time allowed for the connections exceeding the client\-output\-buffer\-limit\-normal\-soft\-limit; 
  + client\-output\-buffer\-limit\-normal\-hard\-limit: A connection hitting this limit will be immediatelly terminated\.

  Besides the regular client buffers, the following options control the buffer for replica nodes and Pub/Sub \(Publish/Subscribe\) clients:
  + client\-output\-buffer\-limit\-replica\-hard\-limit;
  + client\-output\-buffer\-limit\-replica\-soft\-seconds;
  + client\-output\-buffer\-limit\-replica\-hard\-limit;
  + client\-output\-buffer\-limit\-pubsub\-soft\-limit;
  + client\-output\-buffer\-limit\-pubsub\-soft\-seconds;
  + client\-output\-buffer\-limit\-pubsub\-hard\-limit;

## Client\-side troubleshooting for Amazon EC2 instances<a name="Connections-client"></a>

The load and responsiveness on the client side can also affect the requests to ElastiCache\. EC2 instance and operating system limits need to be carefully reviewed while troubleshooting intermittent connectivity or timeout issues\. Some key points to observe:
+ CPU: 
  + EC2 instance CPU usage: Make sure the CPU hasn’t been saturated or near to 100 percent\. Historical analysis can be done via CloudWatch, however keep in mind that data points granularity is either 1 minute \(with detailed monitoring enabled\) or 5 minutes;
  + If using [burstable EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/burstable-performance-instances.html), make sure that their CPU credit balance hasn’t been depleted\. This information is available on the `CPUCreditBalance` CloudWatch metric\.
  + Short periods of high CPU usage can cause timeouts without reflecting on 100 percent utilization on CloudWatch\. Such cases require real\-time monitoring with operating\-system tools like `top`, `ps` and `mpstat`\.
+ Network
  + Check if the Network throughput is under acceptable values according to the instance capabilities\. For more information, see [Amazon EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/)
  + On instances with the `ena` Enhanced Network driver, check the [ena statistics](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/troubleshooting-ena.html#statistics-ena) for timeouts or exceeded limits\. The following statistics are useful to confirm network limits saturation:
    + `bw_in_allowance_exceeded` / `bw_out_allowance_exceeded`: number of packets shaped due to excessive inbound or outbound throughput;
    + `conntrack_allowance_exceeded`: number of packets dropped due to security groups [connection tracking limits](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/security-group-connection-tracking.html#connection-tracking-throttling)\. New connections will fail when this limit is saturated;
    + `linklocal_allowance_exceeded`: number of packets dropped due to excessive requests to instance meta\-data, NTP via VPC DNS\. The limit is 1024 packets per second for all the services;
    + `pps_allowance_exceeded`: number of packets dropped due to excessive packets per second ratio\. The PPS limit can be hit when the network traffic consists on thousands or millions of very small requests per second\. ElastiCache traffic can be optimized to make better use of network packets via pipelines or commands that do multiple operations at once like `MGET` instead of `GET`\.

## Dissecting the time taken to complete a single request<a name="Dissecting-time"></a>
+ On the network: `Tcpdump` and `Wireshark` \(tshark on the command line\) are handy tools to understand how much time the request took to travel the network, hit the ElastiCache engine and get a return\. The following example highlights a single request created with the following command: 

  ```
  $ echo ping | nc example.xxxxxx.ng.0001.use1.cache.amazonaws.com 6379
  +PONG
  ```

  In parallel to the command above, tcpdump was in execution and returned:

  ```
  $ sudo tcpdump -i any -nn port 6379 -tt
  tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
  listening on any, link-type LINUX_SLL (Linux cooked), capture size 262144 bytes
  1609428918.917869 IP 172.31.11.142.40966
      > 172.31.11.247.6379: Flags [S], seq 177032944, win 26883, options [mss 8961,sackOK,TS val 27819440 ecr 0,nop,wscale 7], length 0
  1609428918.918071 IP 172.31.11.247.6379 > 172.31.11.142.40966: Flags [S.], seq 53962565, ack 177032945, win
      28960, options [mss 1460,sackOK,TS val 3788576332 ecr 27819440,nop,wscale 7], length 0
  1609428918.918091 IP 172.31.11.142.40966 > 172.31.11.247.6379: Flags [.], ack 1, win 211, options [nop,nop,TS val 27819440 ecr 3788576332], length 0
  1609428918.918122
      IP 172.31.11.142.40966 > 172.31.11.247.6379: Flags [P.], seq 1:6, ack 1, win 211, options [nop,nop,TS val 27819440 ecr 3788576332], length 5: RESP "ping"
  1609428918.918132 IP 172.31.11.142.40966 > 172.31.11.247.6379: Flags [F.], seq 6, ack
      1, win 211, options [nop,nop,TS val 27819440 ecr 3788576332], length 0
  1609428918.918240 IP 172.31.11.247.6379 > 172.31.11.142.40966: Flags [.], ack 6, win 227, options [nop,nop,TS val 3788576332 ecr 27819440], length 0
  1609428918.918295
      IP 172.31.11.247.6379 > 172.31.11.142.40966: Flags [P.], seq 1:8, ack 7, win 227, options [nop,nop,TS val 3788576332 ecr 27819440], length 7: RESP "PONG"
  1609428918.918300 IP 172.31.11.142.40966 > 172.31.11.247.6379: Flags [.], ack 8, win
      211, options [nop,nop,TS val 27819441 ecr 3788576332], length 0
  1609428918.918302 IP 172.31.11.247.6379 > 172.31.11.142.40966: Flags [F.], seq 8, ack 7, win 227, options [nop,nop,TS val 3788576332 ecr 27819440], length 0
  1609428918.918307
      IP 172.31.11.142.40966 > 172.31.11.247.6379: Flags [.], ack 9, win 211, options [nop,nop,TS val 27819441 ecr 3788576332], length 0
  ^C
  10 packets captured
  10 packets received by filter
  0 packets dropped by kernel
  ```

  From the output above we can confirm that the TCP three\-way handshake was completed in 222 microseconds \(918091 \- 917869\) and the ping command was submitted and returned in 173 microseconds \(918295 \- 918122\)\.

   It took 438 microseconds \(918307 \- 917869\) from requesting to closing the connection\. Those results would confirm that network and engine response times are good and the investigation can focus on other components\.
+ On the operating system: `Strace` can help identifying time gaps on the OS level\. The analysis of actual applications would be way more extensive and specialized application profilers or debuggers are advisable\. The following example just shows if the base operating system components are working as expected, otherwise further investigation may be required\. Using the same Redis `PING` command with `strace` we get:

  ```
  $ echo ping | strace -f -tttt -r -e trace=execve,socket,open,recvfrom,sendto nc example.xxxxxx.ng.0001.use1.cache.amazonaws.com (http://example.xxxxxx.ng.0001.use1.cache.amazonaws.com/)
      6379
  1609430221.697712 (+ 0.000000) execve("/usr/bin/nc", ["nc", "example.xxxxxx.ng.0001.use"..., "6379"], 0x7fffede7cc38 /* 22 vars */) = 0
  1609430221.708955 (+ 0.011231) socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
  1609430221.709084
      (+ 0.000124) socket(AF_UNIX, SOCK_STREAM|SOCK_CLOEXEC|SOCK_NONBLOCK, 0) = 3
  1609430221.709258 (+ 0.000173) open("/etc/nsswitch.conf", O_RDONLY|O_CLOEXEC) = 3
  1609430221.709637 (+ 0.000378) open("/etc/host.conf", O_RDONLY|O_CLOEXEC) = 3
  1609430221.709923
      (+ 0.000286) open("/etc/resolv.conf", O_RDONLY|O_CLOEXEC) = 3
  1609430221.711365 (+ 0.001443) open("/etc/hosts", O_RDONLY|O_CLOEXEC) = 3
  1609430221.713293 (+ 0.001928) socket(AF_INET, SOCK_DGRAM|SOCK_CLOEXEC|SOCK_NONBLOCK, IPPROTO_IP) = 3
  1609430221.717419
      (+ 0.004126) recvfrom(3, "\362|\201\200\0\1\0\2\0\0\0\0\rnotls20201224\6tihew"..., 2048, 0, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("172.31.0.2")}, [28->16]) = 155
  1609430221.717890 (+ 0.000469) recvfrom(3, "\204\207\201\200\0\1\0\1\0\0\0\0\rnotls20201224\6tihew"...,
      65536, 0, {sa_family=AF_INET, sin_port=htons(53), sin_addr=inet_addr("172.31.0.2")}, [28->16]) = 139
  1609430221.745659 (+ 0.027772) socket(AF_INET, SOCK_STREAM, IPPROTO_TCP) = 3
  1609430221.747548 (+ 0.001887) recvfrom(0, 0x7ffcf2f2ca50, 8192,
      0, 0x7ffcf2f2c9d0, [128]) = -1 ENOTSOCK (Socket operation on non-socket)
  1609430221.747858 (+ 0.000308) sendto(3, "ping\n", 5, 0, NULL, 0) = 5
  1609430221.748048 (+ 0.000188) recvfrom(0, 0x7ffcf2f2ca50, 8192, 0, 0x7ffcf2f2c9d0, [128]) = -1 ENOTSOCK
      (Socket operation on non-socket)
  1609430221.748330 (+ 0.000282) recvfrom(3, "+PONG\r\n", 8192, 0, 0x7ffcf2f2c9d0, [128->0]) = 7
  +PONG
  1609430221.748543 (+ 0.000213) recvfrom(3, "", 8192, 0, 0x7ffcf2f2c9d0, [128->0]) = 0
  1609430221.752110
      (+ 0.003569) +++ exited with 0 +++
  ```

   In the example above, the command took a little more than 54 milliseconds to complete \(752110 \- 697712 = 54398 microseconds\)\.

   A significant amount of time, approximately 20ms, was taken to instantiate nc and do the name resolution \(from 697712 to 717890\), after that, 2ms were required to create the TCP socket \(745659 to 747858\), and 0\.4 ms \(747858 to 748330\) to submit and receive the response for the request\. 