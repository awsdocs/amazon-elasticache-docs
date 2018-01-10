# Caching Strategies<a name="Strategies"></a>

This topic covers strategies for populating and maintaining your cache\.

The strategy or strategies you want to implement for populating and maintaining your cache depend upon what data you are caching and the access patterns to that data\. For example, you likely would not want to use the same strategy for both a Top\-10 leaderboard on a gaming site, Facebook posts, and trending news stories\. In the remainder of this section we discuss common cache maintenance strategies, their advantages, and their disadvantages\.


+ [Lazy Loading](#Strategies.LazyLoading)
+ [Write Through](#Strategies.WriteThrough)
+ [Adding TTL](#Strategies.WithTTL)
+ [Related Topics](#Strategies.SeeAlso)

## Lazy Loading<a name="Strategies.LazyLoading"></a>

As the name implies, lazy loading is a caching strategy that loads data into the cache only when necessary\. 

**How Lazy Loading Works**  
Amazon ElastiCache is an in\-memory key/value store that sits between your application and the data store \(database\) that it accesses\. Whenever your application requests data, it first makes the request to the ElastiCache cache\. If the data exists in the cache and is current, ElastiCache returns the data to your application\. If the data does not exist in the cache, or the data in the cache has expired, your application requests the data from your data store which returns the data to your application\. Your application then writes the data received from the store to the cache so it can be more quickly retrieved next time it is requested\.

### Scenario 1: Cache Hit<a name="Strategies.LazyLoading.CacheHit"></a>

**When data is in the cache and isn't expired**

1. Application requests data from the cache\.

1. Cache returns the data to the application\.

### Scenario 2: Cache Miss<a name="Strategies.LazyLoading.CacheMiss"></a>

**When data isn’t in the cache or is expired**

1. Application requests data from the cache\.

1. Cache doesn't have the requested data, so returns a `null`\.

1. Application requests and receives the data from the database\.

1. Application updates the cache with the new data\.

The following diagram illustrates both these processes\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/UserGuide/images/ElastiCache-HowECWorks.png)

### Advantages and Disadvantages of Lazy Loading<a name="Strategies.LazyLoading.Evaluation"></a>

**Advantages of Lazy Loading**

+ Only requested data is cached\.

  Since most data is never requested, lazy loading avoids filling up the cache with data that isn't requested\.

+ Node failures are not fatal\.

  When a node fails and is replaced by a new, empty node the application continues to function, though with increased latency\. As requests are made to the new node each cache miss results in a query of the database and adding the data copy to the cache so that subsequent requests are retrieved from the cache\.

**Disadvantages of Lazy Loading**

+ There is a cache miss penalty\.

  Each cache miss results in 3 trips, 

  1. Initial request for data from the cache

  1. Query of the database for the data

  1. Writing the data to the cache

   which can cause a noticeable delay in data getting to the application\.

+ Stale data\.

  If data is only written to the cache when there is a cache miss, data in the cache can become stale since there are no updates to the cache when data is changed in the database\. This issue is addressed by the [Write Through](#Strategies.WriteThrough) and [Adding TTL](#Strategies.WithTTL) strategies\.

### Lazy Loading Code<a name="Strategies.LazyLoading.CodeExample"></a>

The following code is a pseudo code example of lazy loading logic\.

```
// *****************************************
// function that returns a customer's record.
// Attempts to retrieve the record from the cache.
// If it is retrieved, the record is returned to the application.
// If the record is not retrieved from the cache, it is
//    retrieved from the database, 
//    added to the cache, and 
//    returned to the application
// *****************************************
get_customer(customer_id)

    customer_record = cache.get(customer_id)
    if (customer_record == null)
    
        customer_record = db.query("SELECT * FROM Customers WHERE id == {0}", customer_id)
        cache.set(customer_id, customer_record)
    
    return customer_record
```

The application code that retrieves the data would be:

```
customer_record = get_customer(12345)
```

## Write Through<a name="Strategies.WriteThrough"></a>

The write through strategy adds data or updates data in the cache whenever data is written to the database\.

### Advantages and Disadvantages of Write Through<a name="Strategies.WriteThrough.Evaluation"></a>

**Advantages of Write Through**

+ Data in the cache is never stale\.

  Since the data in the cache is updated every time it is written to the database, the data in the cache is always current\.

+ Write penalty vs\. Read penalty\.

  Every write involves two trips: 

  1. A write to the cache

  1. A write to the database

   Which adds latency to the process\. That said, end users are generally more tolerant of latency when updating data than when retrieving data\. There is an inherent sense that updates are more work and thus take longer\.

**Disadvantages of Write Through**

+ Missing data\.

  In the case of spinning up a new node, whether due to a node failure or scaling out, there is missing data which continues to be missing until it is added or updated on the database\. This can be minimized by implementing [Lazy Loading](#Strategies.LazyLoading) in conjunction with Write Through\.

+ Cache churn\.

  Since most data is never read, there can be a lot of data in the cluster that is never read\. This is a waste of resources\. By [Adding TTL](#Strategies.WithTTL) you can minimize wasted space\.

### Write Through Code<a name="Strategies.WriteThrough.CodeExample"></a>

The following code is a pseudo code example of write through logic\.

```
// *****************************************
// function that saves a customer's record.
// *****************************************
save_customer(customer_id, values)

    customer_record = db.query("UPDATE Customers WHERE id = {0}", customer_id, values)
    cache.set(customer_id, customer_record)
    return success
```

The application code that updates the data would be:

```
save_customer(12345,{"address":"123 Main"})
```

## Adding TTL<a name="Strategies.WithTTL"></a>

Lazy loading allows for stale data, but won't fail with empty nodes\. Write through ensures that data is always fresh, but may fail with empty nodes and may populate the cache with superfluous data\. By adding a time to live \(TTL\) value to each write, we are able to enjoy the advantages of each strategy and largely avoid cluttering up the cache with superfluous data\.

**What is TTL?**  
Time to live \(TTL\) is an integer value that specifies the number of seconds \(Redis can specify seconds or milliseconds\) until the key expires\. When an application attempts to read an expired key, it is treated as though the key is not found, meaning that the database is queried for the key and the cache is updated\. This does not guarantee that a value is not stale, but it keeps data from getting too stale and requires that values in the cache are occasionally refreshed from the database\.

For more information, see the [Redis `set` command](http://redis.io/commands/set) or the [Memcached `set` command\.](http://www.tutorialspoint.com/memcached/memcached_set_data.htm)

### Code Example<a name="Strategies.WithTTL.CodeExample"></a>

The following code is a pseudo code example of write through logic with TTL\.

```
// *****************************************
// function that saves a customer's record.
// The TTL value of 300 means that the record expires
//    300 seconds (5 minutes) after the set command 
//    and future reads will have to query the database.
// *****************************************
save_customer(customer_id, values)

    customer_record = db.query("UPDATE Customers WHERE id = {0}", customer_id, values)
    cache.set(customer_id, customer_record, 300)

    return success
```

The following code is a pseudo code example of lazy loading logic with TTL\.

```
// *****************************************
// function that returns a customer's record.
// Attempts to retrieve the record from the cache.
// If it is retrieved, the record is returned to the application.
// If the record is not retrieved from the cache, it is 
//    retrieved from the database, 
//    added to the cache, and 
//    returned to the application.
// The TTL value of 300 means that the record expires
//    300 seconds (5 minutes) after the set command 
//    and subsequent reads will have to query the database.
// *****************************************
get_customer(customer_id)

    customer_record = cache.get(customer_id)
    
    if (customer_record != null)
        if (customer_record.TTL < 300)
            return customer_record        // return the record and exit function
            
    // do this only if the record did not exist in the cache OR
    //    the TTL was >= 300, i.e., the record in the cache had expired.
    customer_record = db.query("SELECT * FROM Customers WHERE id = {0}", customer_id)
    cache.set(customer_id, customer_record, 300)  // update the cache
    return customer_record                // return the newly retrieved record and exit function
```

The application code would be:

```
save_customer(12345,{"address":"123 Main"})
```

```
customer_record = get_customer(12345)
```

## Related Topics<a name="Strategies.SeeAlso"></a>

+ [What Should I Cache?](UseCases.md#UseCases.DataStore.WhatToCache)

+ [Engines and Versions](SelectEngine.md)

+ [Scaling](Scaling.md)