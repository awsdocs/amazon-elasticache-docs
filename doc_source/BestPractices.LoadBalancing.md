# Configuring Your ElastiCache Client for Efficient Load Balancing<a name="BestPractices.LoadBalancing"></a>

**Note**  
This section applies to multi\-node Memcached clusters\.

To effectively use multiple ElastiCache Memcached nodes, you need to be able to spread your cache keys across the nodes\. A simple way to load balance a cluster with *n* nodes is to calculate the hash of the object’s key and mod the result by *n* \- `hash(key) mod n`\. The resulting value \(0 through *n*–1\) is the number of the node where you place the object\. 

This approach is simple and works well as long as the number of nodes \(*n*\) is constant\. However, whenever you add or remove a node from the cluster, the number of keys that need to be moved is *\(n \- 1\) / n* \(where *n* is the new number of nodes\)\. Thus, this approach results in a large number of keys being moved, which translates to a large number of initial cache misses, especially as the number of nodes gets large\. Scaling from 1 to 2 nodes results in \(2\-1\) / 2 \(50 percent\) of the keys being moved, the best case\. Scaling from 9 to 10 nodes results in \(10–1\)/10 \(90 percent\) of the keys being moved\. If you're scaling up due to a spike in traffic, you don't want to have a large number of cache misses\. A large number of cache misses results in hits to the database, which is already overloaded due to the spike in traffic\.

The solution to this dilemma is consistent hashing\. Consistent hashing uses an algorithm such that whenever a node is added or removed from a cluster, the number of keys that must be moved is roughly *1 / n* \(where *n* is the new number of nodes\)\. Scaling from 1 to 2 nodes results in 1/2 \(50 percent\) of the keys being moved, the worst case\. Scaling from 9 to 10 nodes results in 1/10 \(10 percent\) of the keys being moved\.

As the user, you control which hashing algorithm is used for multi\-node clusters\. We recommend that you configure your clients to use consistent hashing\. Fortunately, there are many Memcached client libraries in most popular languages that implement consistent hashing\. Check the documentation for the library you are using to see if it supports consistent hashing and how to implement it\.

If you are working in Java, PHP, or \.NET, we recommend you use one of the Amazon ElastiCache client libraries\.

## Consistent Hashing Using Java<a name="BestPractices.LoadBalancing.Java"></a>

The ElastiCache Memcached Java client is based on the open\-source spymemcached Java client, which has consistent hashing capabilities built in\. The library includes a KetamaConnectionFactory class that implements consistent hashing\. By default, consistent hashing is turned off in spymemcached\.

For more information, see the KetamaConnectionFactory documentation at [http://dustin\.sallings\.org/java\-memcached\-client/apidocs/net/spy/memcached/KetamaConnectionFactory\.html](http://dustin.sallings.org/java-memcached-client/apidocs/net/spy/memcached/KetamaConnectionFactory.html)\.

## Consistent Hashing Using PHP<a name="BestPractices.LoadBalancing.PHP"></a>

The ElastiCache Memcached PHP client is a wrapper around the built\-in Memcached PHP library\. By default, consistent hashing is turned off by the Memcached PHP library\.

Use the following code to turn on consistent hashing\.

```
$m = new Memcached();
$m->setOption(Memcached::OPT_DISTRIBUTION, Memcached::DISTRIBUTION_CONSISTENT);
```

In addition to the preceding code, we recommend that you also turn `memcached.sess_consistent_hash` on in your php\.ini file\.

 For more information, see the run\-time configuration documentation for Memcached PHP at [http://php\.net/manual/en/memcached\.configuration\.php](http://php.net/manual/en/memcached.configuration.php)\. Note specifically the `memcached.sess_consistent_hash` parameter\.

## Consistent Hashing Using \.NET<a name="BestPractices.LoadBalancing.dotNET"></a>

The ElastiCache Memcached \.NET client is a wrapper around Enyim Memcached\. By default, consistent hashing is turned on by the Enyim Memcached client\.

 For more information, see the `memcached/locator` documentation at [https://github\.com/enyim/EnyimMemcached/wiki/MemcachedClient\-Configuration\#user\-content\-memcachedlocator](https://github.com/enyim/EnyimMemcached/wiki/MemcachedClient-Configuration#user-content-memcachedlocator)\.