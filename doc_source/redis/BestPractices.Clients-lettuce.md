# Lettuce client configuration<a name="BestPractices.Clients-lettuce"></a>

This section describes the recommended Java and Lettuce configuration options, and how they apply to ElastiCache clusters\.

The recommendations in this section were tested with Lettuce version 6\.2\.2\.

**Topics**
+ [Example: Lettuce configuration for cluster mode and TLS enabled](BestPractices.Clients-lettuce-cme.md)
+ [Example: Lettuce configuration for cluster mode disabled and TLS enabled](BestPractices.Clients-lettuce-cmd.md)

**Java DNS cache TTL**

The Java virtual machine \(JVM\) caches DNS name lookups\. When the JVM resolves a hostname to an IP address, it caches the IP address for a specified period of time, known as the *time\-to\-live* \(TTL\)\.

The choice of TTL value is a trade\-off between latency and responsiveness to change\. With shorter TTLs, DNS resolvers notice updates in the cluster's DNS faster\. This can make your application respond faster to replacements or other workflows that your cluster undergoes\. However, if the TTL is too low, it increases the query volume, which can increase the latency of your application\. While there is no correct TTL value, it's worth considering the length of time that you can afford to wait for a change to take effect when setting your TTL value\.

Because ElastiCache nodes use DNS name entries that might change, we recommend that you configure your JVM with a low TTL of 5 to 10 seconds\. This ensures that when a node’s IP address changes, your application will be able to receive and use the resource’s new IP address by requerying the DNS entry\.

On some Java configurations, the JVM default TTL is set so that it will never refresh DNS entries until the JVM is restarted\.

For details on how to set your JVM TTL, see [How to set the JVM TTL](https://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html#how-to-set-the-jvm-ttl)\.

**Lettuce version**

We recommend using Lettuce version 6\.2\.2 or later\.

**Endpoints**

When you're using cluster mode enabled clusters, set the `redisUri` to the cluster configuration endpoint\. The DNS lookup for this URI returns a list of all available nodes in the cluster, and is randomly resolved to one of them during the cluster initialization\. For more details about how topology refresh works, see *dynamicRefreshResources* later in this topic\.

**SocketOption**

Enable [KeepAlive](https://lettuce.io/core/release/api/io/lettuce/core/SocketOptions.KeepAliveOptions.html)\. Enabling this option reduces the need to handle failed connections during command runtime\.

Ensure that you set the [Connection timeout](https://lettuce.io/core/release/api/io/lettuce/core/SocketOptions.Builder.html#connectTimeout-java.time.Duration-) based on your application requirements and workload\. For more information, see the Timeouts section later in this topic\.

**ClusterClientOption: Cluster Mode Enabled client options**

Enable [AutoReconnect](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterClientOptions.Builder.html#autoReconnect-boolean-) when connection is lost\.

Set [CommandTimeout](https://lettuce.io/core/release/api/io/lettuce/core/RedisURI.html#getTimeout--)\. For more details, see the Timeouts section later in this topic\.

Set [nodeFilter](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterClientOptions.Builder.html#nodeFilter-java.util.function.Predicate-) to filter out failed nodes from the topology\. Lettuce saves all nodes that are found in the 'cluster nodes' output \(including nodes with PFAIL/FAIL status\) in the client's 'partitions' \(also known as shards\)\. During the process of creating the cluster topology, it attempts to connect to all the partition nodes\. This Lettuce behavior of adding failed nodes can cause connection errors \(or warnings\) when nodes are getting replaced for any reason\. 

For example, after a failover is finished and the cluster starts the recovery process, while the clusterTopology is getting refreshed, the cluster bus nodes map has a short period of time that the down node is listed as a FAIL node, before it's completely removed from the topology\. During this period, the Lettuce Redis client considers it a healthy node and continually connects to it\. This causes a failure after retrying is exhausted\. 

For example:

```
final ClusterClientOptions clusterClientOptions = 
    ClusterClientOptions.builder()
    ... // other options
    .nodeFilter(it -> 
        ! (it.is(RedisClusterNode.NodeFlag.FAIL) 
        || it.is(RedisClusterNode.NodeFlag.EVENTUAL_FAIL) 
        || it.is(RedisClusterNode.NodeFlag.NOADDR)))
    .validateClusterNodeMembership(false)
    .build();
redisClusterClient.setOptions(clusterClientOptions);
```

**Note**  
Node filtering is best used with DynamicRefreshSources set to true\. Otherwise, if the topology view is taken from a single problematic seed node, that sees a primary node of some shard as failing, it will filter out this primary node, which will result in slots not being covered\. Having multiple seed nodes \(when DynamicRefreshSources is true\) reduces the likelihood of this issue, since at least some of the seed nodes should have an updated topology view after a failover with the newly promoted primary\.

**ClusterTopologyRefreshOptions: Options to control the cluster topology refreshing of the Cluster Mode Enabled client**

Enable [enablePeriodicRefresh](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterTopologyRefreshOptions.Builder.html#enablePeriodicRefresh-java.time.Duration-)\. This enables periodic cluster topology updates so that the client updates the cluster topology in the intervals of the refreshPeriod \(default: 60 seconds\)\. When it's disabled, the client updates the cluster topology only when errors occur when it attempts to run commands against the cluster\. 

With this option enabled, you can reduce the latency that's associated with refreshing the cluster topology by adding this job to a background task\. While topology refreshment is performed in a background job, it can be somewhat slow for clusters with many nodes\. This is because all nodes are being queried for their views to get the most updated cluster view\. If you run a large cluster, you might want to increase the period\.

Enable [enableAllAdaptiveRefreshTriggers](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterTopologyRefreshOptions.Builder.html#enableAllAdaptiveRefreshTriggers--)\. This enables adaptive topology refreshing that uses all [triggers](https://lettuce.io/core/6.1.6.RELEASE/api/io/lettuce/core/cluster/ClusterTopologyRefreshOptions.RefreshTrigger.html): MOVED\_REDIRECT, ASK\_REDIRECT, PERSISTENT\_RECONNECTS, UNCOVERED\_SLOT, UNKNOWN\_NODE\. Adaptive refresh triggers initiate topology view updates based on events that happen during Redis cluster operations\. Enabling this option leads to an immediate topology refresh when one of the preceding triggers occur\. Adaptive triggered refreshes are rate\-limited using a timeout because events can happen on a large scale \(default timeout between updates: 30\)\.

Enable [closeStaleConnections](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterTopologyRefreshOptions.Builder.html#closeStaleConnections-boolean-)\. This enables closing stale connections when refreshing the cluster topology\. It only comes into effect if [ClusterTopologyRefreshOptions\.isPeriodicRefreshEnabled\(\)](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterTopologyRefreshOptions.html#isPeriodicRefreshEnabled--) is true\. When it's enabled, the client can close stale connections and create new ones in the background\. This reduces the need to handle failed connections during command runtime\.

Enable [dynamicRefreshResources](https://lettuce.io/core/release/api/io/lettuce/core/cluster/ClusterTopologyRefreshOptions.Builder.html#dynamicRefreshSources-boolean-)\. We recommend enabling dynamicRefreshResources for small clusters, and disabling it for large clusters\. dynamicRefreshResources enables discovering cluster nodes from the provided seed node \(for example, cluster configuration endpoint\)\. It uses all the discovered nodes as sources for refreshing the cluster topology\. 

Using dynamic refresh queries all discovered nodes for the cluster topology and attempts to choose the most accurate cluster view\. If it's set to false, only the initial seed nodes are used as sources for topology discovery, and the number of clients are obtained only for the initial seed nodes\. When it's disabled, if the cluster configuration endpoint is resolved to a failed node, trying to refresh the cluster view fails and leads to exceptions\. This scenario can happen because it takes some time until a failed node’s entry is removed from the cluster configuration endpoint\. Therefore, the configuration endpoint can still be randomly resolved to a failed node for a short period of time\. 

When it's enabled, however, we use all of the cluster nodes that are received from the cluster view to query for their current view\. Because we filter out failed nodes from that view, the topology refresh will be successful\. However, when dynamicRefreshSources is true, Lettuce queries all nodes to get the cluster view, and then compares the results\. So it can be expensive for clusters with a lot of nodes\. We suggest that you turn off this feature for clusters with many nodes\. 

```
final ClusterTopologyRefreshOptions topologyOptions = 
    ClusterTopologyRefreshOptions.builder()
    .enableAllAdaptiveRefreshTriggers()
    .enablePeriodicRefresh()
    .dynamicRefreshSources(true)
    .build();
```

**ClientResources**

Configure [DnsResolver](https://lettuce.io/core/release/api/io/lettuce/core/resource/DefaultClientResources.Builder.html#dnsResolver-io.lettuce.core.resource.DnsResolver-) with [DirContextDnsResolver](https://lettuce.io/core/release/api/io/lettuce/core/resource/DirContextDnsResolver.html)\. The DNS resolver is based on Java's com\.sun\.jndi\.dns\.DnsContextFactory\.

Configure [reconnectDelay](https://lettuce.io/core/release/api/io/lettuce/core/resource/DefaultClientResources.Builder.html#reconnectDelay-io.lettuce.core.resource.Delay-) with exponential backoff and full jitter\. Lettuce has built\-in retry mechanisms based on the exponential backoff strategies\.\. For details, see [Exponential Backoff and Jitter](https://aws.amazon.com/blogs/architecture/exponential-backoff-and-jitter) on the AWS Architecture Blog\. For more information about the importance of having a retry backoff strategy, see the backoff logic sections of the [Best practices blog post](https://aws.amazon.com/blogs/database/best-practices-redis-clients-and-amazon-elasticache-for-redis/) on the AWS Database Blog\.

```
ClientResources clientResources = DefaultClientResources.builder()
   .dnsResolver(new DirContextDnsResolver())
    .reconnectDelay(
        Delay.fullJitter(
            Duration.ofMillis(100),     // minimum 100 millisecond delay
            Duration.ofSeconds(10),      // maximum 10 second delay
            100, TimeUnit.MILLISECONDS)) // 100 millisecond base
    .build();
```

**Timeouts **

Use a lower connect timeout value than your command timeout\. Lettuce uses lazy connection establishment\. So if the connect timeout is higher than the command timeout, you can have a period of persistent failure after a topology refresh if Lettuce tries to connect to an unhealthy node and the command timeout is always exceeded\. 

Use a dynamic command timeout for different commands\. We recommend that you set the command timeout based on the command expected duration\. For example, use a longer timeout for commands that iterate over several keys, such as FLUSHDB, FLUSHALL, KEYS, SMEMBERS, or Lua scripts\. Use shorter timeouts for single key commands, such as SET, GET, and HSET\.

**Note**  
Timeouts that are configured in the following example are for tests that ran SET/GET commands with keys and values up to 20 bytes long\. The processing time can be longer when the commands are complex or the keys and values are larger\. You should set the timeouts based on the use case of your application\. 

```
private static final Duration META_COMMAND_TIMEOUT = Duration.ofMillis(1000);
private static final Duration DEFAULT_COMMAND_TIMEOUT = Duration.ofMillis(250);
// Socket connect timeout should be lower than command timeout for Lettuce
private static final Duration CONNECT_TIMEOUT = Duration.ofMillis(100);
    
SocketOptions socketOptions = SocketOptions.builder()
    .connectTimeout(CONNECT_TIMEOUT)
    .build();
 

class DynamicClusterTimeout extends TimeoutSource {
     private static final Set<ProtocolKeyword> META_COMMAND_TYPES = ImmutableSet.<ProtocolKeyword>builder()
          .add(CommandType.FLUSHDB)
          .add(CommandType.FLUSHALL)
          .add(CommandType.CLUSTER)
          .add(CommandType.INFO)
          .add(CommandType.KEYS)
          .build();

    private final Duration defaultCommandTimeout;
    private final Duration metaCommandTimeout;

    DynamicClusterTimeout(Duration defaultTimeout, Duration metaTimeout)
    {
        defaultCommandTimeout = defaultTimeout;
        metaCommandTimeout = metaTimeout;
    }

    @Override
    public long getTimeout(RedisCommand<?, ?, ?> command) {
        if (META_COMMAND_TYPES.contains(command.getType())) {
            return metaCommandTimeout.toMillis();
        }
        return defaultCommandTimeout.toMillis();
    }
}

// Use a dynamic timeout for commands, to avoid timeouts during
// cluster management and slow operations.
TimeoutOptions timeoutOptions = TimeoutOptions.builder()
.timeoutSource(
    new DynamicClusterTimeout(DEFAULT_COMMAND_TIMEOUT, META_COMMAND_TIMEOUT))
.build();
```