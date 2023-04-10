# Choosing a network type<a name="network-type"></a>

ElastiCache supports the Internet Protocol versions 4 and 6 \(IPv4 and IPv6\), allowing you to configure your cluster to accept:
+ only IPv4 connections,
+ only IPv6 connections,
+ both IPv4 and IPv6 connections \(dual\-stack\)

IPv6 is supported for workloads using Redis engine version 6\.2 onward on all instances built on the [Nitro system](https://aws.amazon.com/ec2/nitro/)\. There are no additional charges for accessing ElastiCache over IPv6\. 

**Note**  
Migration of clusters created prior to the availability of IPV6 / dual\-stack is not supported\. Switching between network types on newly created clusters is also not supported\.

## Configuring subnets for network type<a name="network-type-subnets"></a>

If you create a cluster in an Amazon VPC, you must specify a subnet group\. ElastiCache uses that subnet group to choose a subnet and IP addresses within that subnet to associate with your nodes\. ElastiCache clusters require a dual\-stack subnet with both IPv4 and IPv6 addresses assigned to them to operate in dual\-stack mode and an IPv6\-only subnet to operate as IPv6\-only\.

## Using dual\-stack<a name="network-type-dual-stack"></a>

When using ElastiCache for Redis in cluster mode enabled, from an application's perspective, connecting to all the cluster nodes through the configuration endpoint is no different than connecting directly to an individual cache node\. To achieve this, a cluster\-aware client must engage in a cluster discovery process and request the configuration information for all nodes\. Redis' discovery protocol supports only one IP per node\. 

To maintain backwards compatibility with all existing clients, IP discovery is introduced, which allows you to select the IP type \(i\.e\., IPv4 or IPv6\) to advertise in the discovery protocol\. While this limits auto discovery to only one IP type, dual\-stack is still beneficial for cluster mode enabled workloads, as it enables migrations \(or rollbacks\) from an IPv4 to an IPv6 Discovery IP type with no downtime\.

## TLS enabled dual stack ElastiCache clusters<a name="configuring-tls-enabled-dual-stack"></a>

When TLS is enabled for ElastiCache clusters the cluster discovery functions \(`cluster slots`, `cluster shards`, and `cluster nodes`\) return hostnames instead of IPs\. The hostnames are then used instead of IPs to connect to the ElastiCache cluster and perform a TLS handshake\. This means that clients won’t be affected by the IP Discovery parameter\. *For TLS enabled clusters the IP Discovery parameter has no effect on the preferred IP protocol\.* Instead, the IP protocol used will be determined by which IP protocol the client prefers when resolving DNS hostnames\.

For examples on how to configure an IP protocol preference when resolving DNS hostnames, see [TLS enabled dual stack ElastiCache clusters](network-type-best-practices.md#network-type-configuring-tls-enabled-dual-stack)\.

## Using the AWS Management Console<a name="network-type-console"></a>

When creating a cluster using the AWS Management Console, under **Connectivity**, choose a network type, either **IPv4**, **IPv6** or **Dual stack**\. If you are creating a Redis \(cluster mode enabled\) cluster and choose dual stack, you then must select a **Discovery IP type**, either IPv6 or IPv4\.

For more information, see [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.md#Clusters.Create.CON.RedisCluster) or [Creating a Redis \(cluster mode disabled\) \(Console\)](Clusters.Create.md#Clusters.Create.CON.Redis)\.

When creating a replication group using the AWS Management Console, choose a network type, either **IPv4**, **IPv6** or **Dual stack**\. If you choose dual stack, you then must select a **Discovery IP type**, either IPv6 or IPv4\.

For more information, see [Creating a Redis \(Cluster Mode Disabled\) replication group from scratch](Replication.CreatingReplGroup.NoExistingCluster.Classic.md) or [Creating a replication group in Redis \(Cluster Mode Enabled\) from scratch](Replication.CreatingReplGroup.NoExistingCluster.Cluster.md)\.

## Using the CLI<a name="network-type-cli"></a>

When creating a cache cluster using the CLI, you use the [create\-cache\-cluster](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-cache-cluster.html) command and specify the `NetworkType` and `IPDiscovery` parameters:

For Linux, macOS, or Unix:

```
aws elasticache create-cache-cluster \
    --cache-cluster-id "cluster-test" \
    --engine redis \
    --cache-node-type cache.m5.large \
    --num-cache-nodes 1 \
    --network-type dual_stack \
    --ip-discovery ipv4
```

For Windows:

```
aws elasticache create-cache-cluster ^
    --cache-cluster-id "cluster-test" ^
    --engine redis ^
    --cache-node-type cache.m5.large ^
    --num-cache-nodes 1 ^
    --network-type dual_stack ^
    --ip-discovery ipv4
```

When creating a replication group with cluster mode disabled using the CLI, you use the [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html) command and specify the `NetworkType` and `IPDiscovery` parameters:

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
   --replication-group-id sample-repl-group \
   --replication-group-description "demo cluster with replicas" \
   --num-cache-clusters 3 \
   --primary-cluster-id redis01 \
   --network-type dual_stack \
   --ip-discovery ipv4
```

For Windows:

```
aws elasticache create-replication-group ^
   --replication-group-id sample-repl-group ^
   --replication-group-description "demo cluster with replicas" ^
   --num-cache-clusters 3 ^
   --primary-cluster-id redis01 ^
   --network-type dual_stack ^
   --ip-discovery ipv4
```

When creating a replication group with cluster mode enabled and use IPv4 for IP discovery using the CLI, you use the [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html) command and specify the `NetworkType` and `IPDiscovery` parameters:

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
  --replication-group-id demo-cluster \
  --replication-group-description "demo cluster" \
  --cache-node-type cache.m5.large \
  --num-node-groups 2 \
  --engine redis \
  --cache-subnet-group-name xyz \
  --network-type dual_stack \
  --ip-discovery ipv4 \
  --region us-east-1
```

For Windows:

```
aws elasticache create-replication-group ^
  --replication-group-id demo-cluster ^
  --replication-group-description "demo cluster" ^
  --cache-node-type cache.m5.large ^
  --num-node-groups 2 ^
  --engine redis ^
  --cache-subnet-group-name xyz ^
  --network-type dual_stack ^
  --ip-discovery ipv4 ^
  --region us-east-1
```

When creating a replication group with cluster mode enabled and use IPv6 for IP discovery using the CLI, you use the [create\-replication\-group](https://docs.aws.amazon.com/cli/latest/reference/elasticache/create-replication-group.html) command and specify the `NetworkType` and `IPDiscovery` parameters:

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
  --replication-group-id demo-cluster \
  --replication-group-description "demo cluster" \
  --cache-node-type cache.m5.large \
  --num-node-groups 2 \
  --engine redis \
  --cache-subnet-group-name xyz \
  --network-type dual_stack \
  --ip-discovery ipv6 \
  --region us-east-1
```

For Windows:

```
aws elasticache create-replication-group ^
  --replication-group-id demo-cluster ^
  --replication-group-description "demo cluster" ^
  --cache-node-type cache.m5.large ^
  --num-node-groups 2 ^
  --engine redis ^
  --cache-subnet-group-name xyz ^
  --network-type dual_stack ^
  --ip-discovery ipv6 ^
  --region us-east-1
```