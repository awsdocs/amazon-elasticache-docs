# Memcached Specific Parameters<a name="ParameterGroups.Memcached"></a>

If you do not specify a parameter group for your Memcached cluster, then a default parameter group appropriate to your engine version will be used\. You cannot change the values of any parameters in a default parameter group; however, you can create a custom parameter group and assign it to your cluster at any time\. For more information, see [Creating a Parameter Group](ParameterGroups.Creating.md)\.

**Topics**
+ [Memcached 1\.5\.10 Parameter Changes](#ParameterGroups.Memcached.1-5-10)
+ [Memcached 1\.4\.34 Added Parameters](#ParameterGroups.Memcached.1-4-34)
+ [Memcached 1\.4\.33 Added Parameters](#ParameterGroups.Memcached.1-4-33)
+ [Memcached 1\.4\.24 Added Parameters](#ParameterGroups.Memcached.1-4-24)
+ [Memcached 1\.4\.14 Added Parameters](#ParameterGroups.Memcached.1-4-14)
+ [Memcached 1\.4\.5 Supported Parameters](#ParameterGroups.Memcached.1-4-5)
+ [Memcached Connection Overhead](#ParameterGroups.Memcached.Overhead)
+ [Memcached Node\-Type Specific Parameters](#ParameterGroups.Memcached.NodeSpecific)

## Memcached 1\.5\.10 Parameter Changes<a name="ParameterGroups.Memcached.1-5-10"></a>

For Memcached 1\.5\.10, the following additional parameters are supported\.

**Parameter group family:** memcached1\.5


| Name | Details | Description | 
| --- | --- | --- | 
| no\_modern  | Default: 0 Type: boolean Modifiable: Yes Allowed\_Values: 0,1 Changes Take Effect: At launch  |  An alias for disabling `slab_reassign`, `slab_automove`, `lru_crawler`, `lru_maintainer`, `maxconns_fast` commands\. `No modern` also sets the hash\_algorithm to `jenkins` and allows inlining of ASCII VALUE\. Applicable to memcached 1\.5 and higher\. To revert to `modern`, which is now the default, you must re\-launch\. | 
| inline\_ascii\_resp  | Default: 0 Type: boolean Modifiable: Yes Allowed\_Values: 0,1 Changes Take Effect: At launch  |  Stores numbers from `VALUE` response, inside an item, using up to 24 bytes\. Small slowdown for ASCII `get`, `faster` sets\.  | 

For Memcached 1\.5\.10, the following parameters are removed\.


| Name | Details | Description | 
| --- | --- | --- | 
| expirezero\_does\_not\_evict  | Default: 0 Type: boolean Modifiable: Yes Allowed\_Values: 0,1 Changes Take Effect: At launch  |  No longer supported in this version\. | 
| modern  | Default: 1 Type: boolean Modifiable: Yes \(requires re\-launch if set to `no-modern`\) Allowed\_Values: 0,1 Changes Take Effect: At launch  |  No longer supported in this version\. Starting with this version, `modern` is enabled by default with every launch or re\-launch\.  | 

## Memcached 1\.4\.34 Added Parameters<a name="ParameterGroups.Memcached.1-4-34"></a>

For Memcached 1\.4\.34, no additional parameters are supported\.

**Parameter group family:** memcached1\.4

## Memcached 1\.4\.33 Added Parameters<a name="ParameterGroups.Memcached.1-4-33"></a>

For Memcached 1\.4\.33, the following additional parameters are supported\.

**Parameter group family:** memcached1\.4


| Name | Details | Description | 
| --- | --- | --- | 
|  modern  | Default: enabled Type: boolean Modifiable: Yes Changes Take Effect: At launch  |  An alias to multiple features\. Enabling `modern` is equivalent to turning following commands on and using a murmur3 hash algorithm: `slab_reassign`, `slab_automove`, `lru_crawler`, `lru_maintainer`, `maxconns_fast`, and `hash_algorithm=murmur3`\. | 
|  watch  | Default: enabled Type: boolean Modifiable: Yes Changes Take Effect: Immediately Logs can get dropped if user hits their `watcher_logbuf_size` and `worker_logbuf_size` limits\.  |  Logs fetches, evictions or mutations\. When, for example, user turns `watch` on, they can see logs when `get`, `set`, `delete`, or `update` occur\. | 
|  idle\_timeout  | Default: 0 \(disabled\) Type: integer Modifiable: Yes Changes Take Effect: At Launch  |  The minimum number of seconds a client will be allowed to idle before being asked to close\. Range of values: 0 to 86400\. | 
|  cache\_memlimit  | Type: integer Modifiable: Yes Changes Take Effect: Immediately  |  If memory isn't being preallocated, allows dynamically increasing the memory limit of a running system\. `cache_memlimit N` where N is a value in megabytes\. Value can go up or down\. Range: 8 \(MB\) to the node type's `maxmemory`\. | 
|  track\_sizes  | Default: disabled Type: boolean Modifiable: Yes Changes Take Effect: At Launch  |  Shows the sizes each slab group has consumed\. Enabling `track_sizes` lets you run `stats sizes` without the need to run `stats sizes_enable`\. | 
|  watcher\_logbuf\_size  | Default: 256 \(KB\) Type: integer Modifiable: Yes Changes Take Effect: At Launch  |  The `watch` command turns on stream logging for Memcached\. However `watch` can drop logs if the rate of evictions, mutations or fetches are high enough to cause the logging buffer to become full\. In such situations, users can increase the buffer size to reduce the chance of log losses\. | 
|  worker\_logbuf\_size  | Default: 64 \(KB\) Type: integer Modifiable: Yes Changes Take Effect: At Launch  |  The `watch` command turns on stream logging for Memcached\. However `watch` can drop logs if the rate of evictions, mutations or fetches are high enough to cause logging buffer get full\. In such situations, users can increase the buffer size to reduce the chance of log losses\. | 
|  slab\_chunk\_max  | Default: 524288 \(bytes\)  Type: integer Modifiable: Yes Changes Take Effect: At Launch  |  Specifies the maximum size of a slab\. Setting smaller slab size uses memory more efficiently\. Items larger than `slab_chunk_max` are split over multiple slabs\. | 
|  lru\_crawler metadump \[all\|1\|2\|3\] | Default: disabled  Type: boolean Modifiable: Yes Changes Take Effect: Immediately  |  if lru\_crawler is enabled this command dumps all keys\. `all\|1\|2\|3` \- all slabs, or specify a particular slab number | 

## Memcached 1\.4\.24 Added Parameters<a name="ParameterGroups.Memcached.1-4-24"></a>

For Memcached 1\.4\.24, the following additional parameters are supported\.

**Parameter group family:** memcached1\.4


| Name | Details | Description | 
| --- | --- | --- | 
|  disable\_flush\_all  | Default: 0 \(disabled\) Type: boolean Modifiable: Yes Changes Take Effect: At launch  |  Add parameter \(`-F`\) to disable flush\_all\. Useful if you never want to be able to run a full flush on production instances\. Values: 0, 1 \(user can do a `flush_all` when the value is 0\)\. | 
|  hash\_algorithm  | Default: jenkins Type: string Modifiable: Yes Changes Take Effect: At launch  | The hash algorithm to be used\. Permitted values: murmur3 and jenkins\. | 
|  lru\_crawler  | Default: 0 \(disabled\) Type: boolean Modifiable: Yes Changes Take Effect: After restart You can temporarily enable `lru_crawler` at runtime from the command line\. For more information, see the Description column\.  |  Cleans slab classes of items that have expired\. This is a low impact process that runs in the background\. Currently requires initiating a crawl using a manual command\. To temporarily enable, run `lru_crawler enable` at the command line\. `lru_crawler 1,3,5` crawls slab classes 1, 3, and 5 looking for expired items to add to the freelist\. Values: 0,1 Enabling `lru_crawler` at the command line enables the crawler until either disabled at the command line or the next reboot\. To enable permanently, you must modify the parameter value\. For more information, see [Modifying a Parameter Group](ParameterGroups.Modifying.md)\.  | 
|  lru\_maintainer  | Default: 0 \(disabled\) Type: boolean Modifiable: Yes Changes Take Effect: At launch  |  A background thread that shuffles items between the LRUs as capacities are reached\. Values: 0, 1\.  | 
|  expirezero\_does\_not\_evict  | Default: 0 \(disabled\) Type: boolean Modifiable: Yes Changes Take Effect: At launch  |  When used with `lru_maintainer`, makes items with an expiration time of 0 unevictable\.  This can crowd out memory available for other evictable items\.  Can be set to disregard `lru_maintainer`\. | 

## Memcached 1\.4\.14 Added Parameters<a name="ParameterGroups.Memcached.1-4-14"></a>

For Memcached 1\.4\.14, the following additional parameters are supported\.

**Parameter group family:** memcached1\.4


**Parameters added in Memcached 1\.4\.14**  

|  Name  |  Details  |  Description  | 
| --- | --- | --- | 
| config\_max | Default: 16 Type: integer Modifiable: No | The maximum number of ElastiCache configuration entries\. | 
| config\_size\_max | Default: 65536 Type: integer Modifiable: No | The maximum size of the configuration entries, in bytes\. | 
| hashpower\_init | Default: 16 Type: integer Modifiable: No | The initial size of the ElastiCache hash table, expressed as a power of two\. The default is 16 \(2^16\), or 65536 keys\. | 
| maxconns\_fast | Default: 0 \(false\) Type: Boolean Modifiable: Yes Changes Take Effect: After restart | Changes the way in which new connections requests are handled when the maximum connection limit is reached\. If this parameter is set to 0 \(zero\), new connections are added to the backlog queue and will wait until other connections are closed\. If the parameter is set to 1, ElastiCache sends an error to the client and immediately closes the connection\. | 
| slab\_automove | Default: 0 Type: integer Modifiable: Yes Changes Take Effect: After restart | Adjusts the slab automove algorithm: If this parameter is set to 0 \(zero\), the automove algorithm is disabled\. If it is set to 1, ElastiCache takes a slow, conservative approach to automatically moving slabs\. If it is set to 2, ElastiCache aggressively moves slabs whenever there is an eviction\. \(This mode is not recommended except for testing purposes\.\) | 
| slab\_reassign | Default: 0 \(false\) Type: Boolean Modifiable: Yes Changes Take Effect: After restart | Enable or disable slab reassignment\. If this parameter is set to 1, you can use the "slabs reassign" command to manually reassign memory\. | 

## Memcached 1\.4\.5 Supported Parameters<a name="ParameterGroups.Memcached.1-4-5"></a>

**Parameter group family:** memcached1\.4

For Memcached 1\.4\.5, the following parameters are supported\.


**Parameters added in Memcached 1\.4\.5**  

|  Name  |  Details  |  Description  | 
| --- | --- | --- | 
| backlog\_queue\_limit | Default: 1024 Type: integer Modifiable: No | The backlog queue limit\. | 
| binding\_protocol | Default: auto Type: string Modifiable: Yes Changes Take Effect: After restart | The binding protocol\. Permissible values are: `ascii` and `auto`\. For guidance on modifying the value of `binding_protocol`, see [Modifying a Parameter Group](ParameterGroups.Modifying.md)\. | 
| cas\_disabled | Default: 0 \(false\) Type: Boolean Modifiable: Yes Changes Take Effect: After restart | If 1 \(true\), check and set \(CAS\) operations will be disabled, and items stored will consume 8 fewer bytes than with CAS enabled\. | 
| chunk\_size | Default: 48 Type: integer Modifiable: Yes Changes Take Effect: After restart | The minimum amount, in bytes, of space to allocate for the smallest item's key, value, and flags\. | 
| chunk\_size\_growth\_factor | Default: 1\.25 Type: float Modifiable: Yes Changes Take Effect: After restart | The growth factor that controls the size of each successive Memcached chunk; each chunk will be chunk\_size\_growth\_factor times larger than the previous chunk\. | 
| error\_on\_memory\_exhausted | Default: 0 \(false\) Type: Boolean Modifiable: Yes Changes Take Effect: After restart | If 1 \(true\), when there is no more memory to store items, Memcached will return an error rather than evicting items\. | 
| large\_memory\_pages | Default: 0 \(false\) Type: Boolean Modifiable: No | If 1 \(true\), ElastiCache will try to use large memory pages\. | 
| lock\_down\_paged\_memory | Default: 0 \(false\) Type: Boolean Modifiable: No | If 1 \(true\), ElastiCache will lock down all paged memory\. | 
| max\_item\_size | Default: 1048576 Type: integer Modifiable: Yes Changes Take Effect: After restart | The size, in bytes, of the largest item that can be stored in the cluster\. | 
| max\_simultaneous\_connections | Default: 65000 Type: integer Modifiable: No | The maximum number of simultaneous connections\. | 
| maximize\_core\_file\_limit | Default: 0 \(false\) Type: Boolean Modifiable:  Changes Take Effect: No | If 1 \(true\), ElastiCache will maximize the core file limit\. | 
| memcached\_connections\_overhead | Default: 100 Type: integer Modifiable: Yes Changes Take Effect: After restart | The amount of memory to be reserved for Memcached connections and other miscellaneous overhead\. For information about this parameter, see [Memcached Connection Overhead](#ParameterGroups.Memcached.Overhead)\. | 
| requests\_per\_event | Default: 20 Type: integer Modifiable: No | The maximum number of requests per event for a given connection\. This limit is required to prevent resource starvation\. | 

## Memcached Connection Overhead<a name="ParameterGroups.Memcached.Overhead"></a>

On each node, the memory made available for storing items is the total available memory on that node \(which is stored in the `max_cache_memory` parameter\) minus the memory used for connections and other overhead \(which is stored in the `memcached_connections_overhead` parameter\)\. For example, a node of type `cache.m1.small` has a `max_cache_memory` of 1300MB\. With the default `memcached_connections_overhead` value of 100MB, the Memcached process will have 1200MB available to store items\.

The default values for the `memcached_connections_overhead` parameter satisfy most use cases; however, the required amount of allocation for connection overhead can vary depending on multiple factors, including request rate, payload size, and the number of connections\.

You can change the value of the `memcached_connections_overhead` to better suit the needs of your application\. For example, increasing the value of the `memcached_connections_overhead` parameter will reduce the amount of memory available for storing items and provide a larger buffer for connection overhead\. Decreasing the value of the `memcached_connections_overhead` parameter will give you more memory to store items, but can increase your risk of swap usage and degraded performance\. If you observe swap usage and degraded performance, try increasing the value of the `memcached_connections_overhead` parameter\.

**Important**  
For the `cache.t1.micro` node type, the value for `memcached_connections_overhead` is determined as follows:  
If you cluster is using the default parameter group, ElastiCache will set the value for `memcached_connections_overhead` to 13MB\.
If your cluster is using a parameter group that you have created yourself, you can set the value of `memcached_connections_overhead` to a value of your choice\.

## Memcached Node\-Type Specific Parameters<a name="ParameterGroups.Memcached.NodeSpecific"></a>

Although most parameters have a single value, some parameters have different values depending on the node type used\. The following table shows the default values for the `max_cache_memory` and `num_threads` parameters for each node type\. The values on these parameters cannot be modified\.


**Node Type\-Specific Parameters**  

| Node Type | max\_cache\_memory \(MiB\) | num\-threads  | 
| --- | --- | --- | 
| cache\.t1\.micro | 213 | 1 | 
| cache\.t2\.micro | 555 | 1 | 
| cache\.t2\.small | 1588 | 1 | 
| cache\.t2\.medium | 3301 | 2 | 
| cache\.m1\.small | 1300 | 1 | 
| cache\.m1\.medium | 3350 | 1 | 
| cache\.m1\.large | 7100 | 2 | 
| cache\.m1\.xlarge | 14600 | 4 | 
| cache\.m2\.xlarge | 16700 | 2 | 
| cache\.m2\.2xlarge | 33800 | 4 | 
| cache\.m2\.4xlarge | 68000 | 8 | 
| cache\.m3\.medium | 2850 | 1 | 
| cache\.m3\.large | 6200 | 2 | 
| cache\.m3\.xlarge | 13600 | 4 | 
| cache\.m3\.2xlarge | 28600 | 8 | 
| cache\.m4\.large | 6573 | 2 | 
| cache\.m4\.xlarge | 14618 | 4 | 
| cache\.m4\.2xlarge | 30412 | 8 | 
| cache\.m4\.4xlarge | 62234 | 16 | 
| cache\.m4\.10xlarge | 158355 | 40 | 
| cache\.c1\.xlarge | 6600 | 8 | 
| cache\.r3\.large | 13800 | 2 | 
| cache\.r3\.xlarge | 29100 | 4 | 
| cache\.r3\.2xlarge | 59600 | 8 | 
| cache\.r3\.4xlarge | 120600 | 16 | 
| cache\.r3\.8xlarge | 242600 | 32 | 
| cache\.r4\.large | 12590 | 2 | 
| cache\.r4\.xlarge | 25652 | 4 | 
| cache\.r4\.2xlarge | 51686 | 8 | 
| cache\.r4\.4xlarge | 103815 | 16 | 
| cache\.r4\.8xlarge | 208144 | 32 | 
| cache\.r4\.16xlarge | 416776 | 64 | 

**Note**  
All T2 instances are created in an Amazon Virtual Private Cloud \(Amazon VPC\)\.