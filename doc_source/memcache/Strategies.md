# Caching strategies<a name="Strategies"></a>

In the following topic, you can find strategies for populating and maintaining your cache\.

What strategies to implement for populating and maintaining your cache depend upon what data you cache and the access patterns to that data\. For example, you likely don't want to use the same strategy for both a top\-10 leaderboard on a gaming site and trending news stories\. In the rest of this section, we discuss common cache maintenance strategies and their advantages and disadvantages\.

**Topics**
+ [Lazy loading](#Strategies.LazyLoading)
+ [Write\-through](#Strategies.WriteThrough)
+ [Adding TTL](#Strategies.WithTTL)
+ [Related topics](#Strategies.SeeAlso)

## Lazy loading<a name="Strategies.LazyLoading"></a>

As the name implies, *lazy loading* is a caching strategy that loads data into the cache only when necessary\. It works as described following\. 

Amazon ElastiCache is an in\-memory key\-value store that sits between your application and the data store \(database\) that it accesses\. Whenever your application requests data, it first makes the request to the ElastiCache cache\. If the data exists in the cache and is current, ElastiCache returns the data to your application\. If the data doesn't exist in the cache or has expired, your application requests the data from your data store\. Your data store then returns the data to your application\. Your application next writes the data received from the store to the cache\. This way, it can be more quickly retrieved the next time it's requested\.

A *cache hit* occurs when data is in the cache and isn't expired:

1. Your application requests data from the cache\.

1. The cache returns the data to the application\.

A *cache miss* occurs when data isn't in the cache or is expired:

1. Your application requests data from the cache\.

1. The cache doesn't have the requested data, so returns a `null`\.

1. Your application requests and receives the data from the database\.

1. Your application updates the cache with the new data\.

### Advantages and disadvantages of lazy loading<a name="Strategies.LazyLoading.Evaluation"></a>

The advantages of lazy loading are as follows:
+ Only requested data is cached\.

  Because most data is never requested, lazy loading avoids filling up the cache with data that isn't requested\.
+ Node failures aren't fatal for your application\.

  When a node fails and is replaced by a new, empty node, your application continues to function, though with increased latency\. As requests are made to the new node, each cache miss results in a query of the database\. At the same time, the data copy is added to the cache so that subsequent requests are retrieved from the cache\.

The disadvantages of lazy loading are as follows:
+ There is a cache miss penalty\. Each cache miss results in three trips: 

  1. Initial request for data from the cache

  1. Query of the database for the data

  1. Writing the data to the cache

   These misses can cause a noticeable delay in data getting to the application\.
+ Stale data\.

  If data is written to the cache only when there is a cache miss, data in the cache can become stale\. This result occurs because there are no updates to the cache when data is changed in the database\. To address this issue, you can use the [Write\-through](#Strategies.WriteThrough) and [Adding TTL](#Strategies.WithTTL) strategies\.

### Lazy loading pseudocode example<a name="Strategies.LazyLoading.CodeExample"></a>

The following is a pseudocode example of lazy loading logic\.

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
    
        customer_record = db.query("SELECT * FROM Customers WHERE id = {0}", customer_id)
        cache.set(customer_id, customer_record)
    
    return customer_record
```

For this example, the application code that gets the data is the following\.

```
customer_record = get_customer(12345)
```

## Write\-through<a name="Strategies.WriteThrough"></a>

The write\-through strategy adds data or updates data in the cache whenever data is written to the database\.

### Advantages and disadvantages of write\-through<a name="Strategies.WriteThrough.Evaluation"></a>

The advantages of write\-through are as follows:
+ Data in the cache is never stale\.

  Because the data in the cache is updated every time it's written to the database, the data in the cache is always current\.
+ Write penalty vs\. read penalty\.

  Every write involves two trips: 

  1. A write to the cache

  1. A write to the database

   Which adds latency to the process\. That said, end users are generally more tolerant of latency when updating data than when retrieving data\. There is an inherent sense that updates are more work and thus take longer\.

The disadvantages of write\-through are as follows:
+ Missing data\.

  If you spin up a new node, whether due to a node failure or scaling out, there is missing data\. This data continues to be missing until it's added or updated on the database\. You can minimize this by implementing [lazy loading](#Strategies.LazyLoading) with write\-through\.
+ Cache churn\.

  Most data is never read, which is a waste of resources\. By [adding a time to live \(TTL\) value](#Strategies.WithTTL), you can minimize wasted space\.

### Write\-through pseudocode example<a name="Strategies.WriteThrough.CodeExample"></a>

The following is a pseudocode example of write\-through logic\.

```
// *****************************************
// function that saves a customer's record.
// *****************************************
save_customer(customer_id, values)

    customer_record = db.query("UPDATE Customers WHERE id = {0}", customer_id, values)
    cache.set(customer_id, customer_record)
    return success
```

For this example, the application code that gets the data is the following\.

```
save_customer(12345,{"address":"123 Main"})
```

## Adding TTL<a name="Strategies.WithTTL"></a>

Lazy loading allows for stale data but doesn't fail with empty nodes\. Write\-through ensures that data is always fresh, but can fail with empty nodes and can populate the cache with superfluous data\. By adding a time to live \(TTL\) value to each write, you can have the advantages of each strategy\. At the same time, you can and largely avoid cluttering up the cache with extra data\.


*Time to live \(TTL\)* is an integer value that specifies the number of seconds until the key expires\. Memcached specifies this value in seconds\. When an application attempts to read an expired key, it is treated as though the key is not found\. The database is queried for the key and the cache is updated\. This approach doesn't guarantee that a value isn't stale\. However, it keeps data from getting too stale and requires that values in the cache are occasionally refreshed from the database\.

For more information, see the [Memcached `set` command](http://www.tutorialspoint.com/memcached/memcached_set_data.htm)\.

### TTL pseudocode examples<a name="Strategies.WithTTL.CodeExample"></a>

The following is a pseudocode example of write\-through logic with TTL\.

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

The following is a pseudocode example of lazy loading logic with TTL\.

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

For this example, the application code that gets the data is the following\.

```
save_customer(12345,{"address":"123 Main"})
```

```
customer_record = get_customer(12345)
```

## Related topics<a name="Strategies.SeeAlso"></a>
+ [In\-Memory Data Store](elasticache-use-cases.md#elasticache-use-cases-data-store)
+ [Choosing an Engine and Version](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/SelectEngine.html)
+ [Scaling ElastiCache for Memcached clusters](Scaling.md)