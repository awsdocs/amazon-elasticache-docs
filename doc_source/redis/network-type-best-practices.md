# IPv6 client examples<a name="network-type-best-practices"></a>

Following are best practices for interacting with IPv6 enabled ElastiCache resources with commonly used open\-source client libraries\. You can view [existing best practices for interacting with ElastiCache](http://aws.amazon.com/blogs/database/best-practices-redis-clients-and-amazon-elasticache-for-redis/) for recommendations on configuring clients for ElastiCache resources\. However, there are some caveats worth noting when interacting with IPv6 enabled resources\.

## Validated clients<a name="network-type-validated-clients-redis"></a>

ElastiCache is compatible with open\-source Redis\. This means that open source Redis clients that support IPv6 connections should be able to connect to IPv6 enabled ElastiCache for Redis clusters\. In addition, several of the most popular Python and Java clients have been specifically tested and validated to work with all supported network type configurations \(IPv4 only, IPv6 only, and Dual Stack\)

Validated Clients:
+ [Redis Py \(\)](https://github.com/redis/redis-py) – [4\.1\.2](https://github.com/redis/redis-py/tree/v4.1.2)
+ [Lettuce](https://lettuce.io/) – [Version: 6\.1\.6\.RELEASE](https://github.com/lettuce-io/lettuce-core/tree/6.1.6.RELEASE)
+ [Jedis](https://github.com/redis/jedis) – [Version: 3\.6\.0](https://github.com/redis/jedis/tree/jedis-3.6.0)

## Configuring a preferred protocol for dual stack clusters<a name="network-type-configuring-dual-stack-redis"></a>

For cluster mode enabled Redis clusters, you can control the protocol clients will use to connect to the nodes in the cluster with the IP Discovery parameter\. The IP Discovery parameter can be set to either IPv4 or IPv6\. 

For Redis clusters, the IP discovery parameter sets the IP protocol used in the [cluster slots \(\)](https://redis.io/commands/cluster-slots/), [cluster shards \(\)](https://redis.io/commands/cluster-shards/), and [cluster nodes \(\)](https://redis.io/commands/cluster-nodes/) output\. These commands are used by clients to discover the cluster topology\. Clients use the IPs in theses commands to connect to the other nodes in the cluster\. 

Changing the IP Discovery will not result in any downtime for connected clients\. However, the changes will take some time to propagate\. To determine when the changes have completely propagated for a Redis Cluster, monitor the output of `cluster slots`\. Once all of the nodes returned by the cluster slots command report IPs with the new protocol the changes have finished propagating\. 

Example with Redis\-Py:

```
cluster = RedisCluster(host="xxxx", port=6379)
target_type = IPv6Address # Or IPv4Address if changing to IPv4

nodes = set()
while len(nodes) == 0 or not all((type(ip_address(host)) is target_type) for host in nodes):
    nodes = set()

   # This refreshes the cluster topology and will discovery any node updates.
   # Under the hood it calls cluster slots
    cluster.nodes_manager.initialize()
    for node in cluster.get_nodes():
        nodes.add(node.host)
    self.logger.info(nodes)

    time.sleep(1)
```

Example with Lettuce:

```
RedisClusterClient clusterClient = RedisClusterClient.create(RedisURI.create("xxxx", 6379));

Class targetProtocolType = Inet6Address.class; // Or Inet4Address.class if you're switching to IPv4

Set<String> nodes;
    
do {
   // Check for any changes in the cluster topology.
   // Under the hood this calls cluster slots
    clusterClient.refreshPartitions();
    Set<String> nodes = new HashSet<>();

    for (RedisClusterNode node : clusterClient.getPartitions().getPartitions()) {
        nodes.add(node.getUri().getHost());
    }

    Thread.sleep(1000);
} while (!nodes.stream().allMatch(node -> {
            try {
                return finalTargetProtocolType.isInstance(InetAddress.getByName(node));
            } catch (UnknownHostException ignored) {}
            return false;
}));
```

## TLS enabled dual stack ElastiCache clusters<a name="network-type-configuring-tls-enabled-dual-stack"></a>

When TLS is enabled for ElastiCache clusters the cluster discovery functions \(`cluster slots`, `cluster shards`, and `cluster nodes`\) return hostnames instead of IPs\. The hostnames are then used instead of IPs to connect to the ElastiCache cluster and perform a TLS handshake\. This means that clients won’t be affected by the IP Discovery parameter\. For TLS enabled clusters the IP Discovery parameter has no effect on the preferred IP protocol\. Instead, the IP protocol used will be determined by which IP protocol the client prefers when resolving DNS hostnames\.

**Java clients**

When connecting from a Java environment that supports both IPv4 and IPv6, Java will by default prefer IPv4 over IPv6 for backwards compatibility\. However, the IP protocol preference is configurable through the JVM arguments\. To prefer IPv4, the JVM accepts `-Djava.net.preferIPv4Stack=true` and to prefer IPv6 set `-Djava.net.preferIPv6Stack=true`\. Setting `-Djava.net.preferIPv4Stack=true` means that the JVM will no longer make any IPv6 connections\. **Including those to other non Redis applications\.**

**Host Level Preferences**

In general, if the client or client runtime don’t provide configuration options for setting an IP protocol preference, when performing DNS resolution, the IP protocol will depend on the host’s configuration\. By default, most hosts prefer IPv6 over IPv4 but this preference can be configured at the host level\. This will affect all DNS requests from that host, not just those to ElastiCache clusters\.

**Linux hosts**

For Linux, an IP protocol preference can be configured by modifying the `gai.conf` file\. The `gai.conf` file can be found under `/etc/gai.conf`\. If there is no `gai.conf` specified then an example one should be available under `/usr/share/doc/glibc-common-x.xx/gai.conf` which can be copied to `/etc/gai.conf` and then the default configuration should be un\-commented\. To update the configuration to prefer IPv4 when connecting to an ElastiCache cluster update the precedence for the CIDR range encompassing the cluster IPs to be above the precedence for default IPv6 connections\. By default IPv6 connections have a precedence of 40\. For example, assuming the cluster is located in a subnet with the CIDR 172\.31\.0\.0:0/16, the configuration below would cause clients to prefer IPv4 connections to that cluster\.

```
label ::1/128       0
label ::/0          1
label 2002::/16     2
label ::/96         3
label ::ffff:0:0/96 4
label fec0::/10     5
label fc00::/7      6
label 2001:0::/32   7
label ::ffff:172.31.0.0/112 8
#
#    This default differs from the tables given in RFC 3484 by handling
#    (now obsolete) site-local IPv6 addresses and Unique Local Addresses.
#    The reason for this difference is that these addresses are never
#    NATed while IPv4 site-local addresses most probably are.  Given
#    the precedence of IPv6 over IPv4 (see below) on machines having only
#    site-local IPv4 and IPv6 addresses a lookup for a global address would
#    see the IPv6 be preferred.  The result is a long delay because the
#    site-local IPv6 addresses cannot be used while the IPv4 address is
#    (at least for the foreseeable future) NATed.  We also treat Teredo
#    tunnels special.
#
# precedence  <mask>   <value>
#    Add another rule to the RFC 3484 precedence table.  See section 2.1
#    and 10.3 in RFC 3484.  The default is:
#
precedence  ::1/128       50
precedence  ::/0          40
precedence  2002::/16     30
precedence ::/96          20
precedence ::ffff:0:0/96  10
precedence ::ffff:172.31.0.0/112 100
```

More details on `gai.conf` are available on the [Linux main page](https://man7.org/linux/man-pages/man5/gai.conf.5.html) 

**Windows hosts**

The process for Windows hosts is similar\. For Windows hosts you can run `netsh interface ipv6 set prefix CIDR_CONTAINING_CLUSTER_IPS PRECEDENCE LABEL`\. This has the same effect as modifying the `gai.conf` file on Linux hosts\.

This will update the preference policies to prefer IPv4 connections over IPv6 connections for the specified CIDR range\. For example, assuming that the cluster is in a subnet with the 172\.31\.0\.0:0/16 CIDR executing `netsh interface ipv6 set prefix ::ffff:172.31.0.0:0/112 100 15` would result in the following precedence table which would cause clients to prefer IPv4 when connecting to the cluster\. 

```
C:\Users\Administrator>netsh interface ipv6 show prefixpolicies
Querying active state...

Precedence Label Prefix
---------- ----- --------------------------------
100 15 ::ffff:172.31.0.0:0/112
20 4 ::ffff:0:0/96
50 0 ::1/128
40 1 ::/0
30 2 2002::/16
5 5 2001::/32
3 13 fc00::/7
1 11 fec0::/10
1 12 3ffe::/16
1 3 ::/96
```