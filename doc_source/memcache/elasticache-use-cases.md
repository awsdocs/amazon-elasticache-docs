# Common ElastiCache Use Cases and How ElastiCache Can Help<a name="elasticache-use-cases"></a>

Whether serving the latest news or a product catalog, or selling tickets to an event, speed is the name of the game\. The success of your website and business is greatly affected by the speed at which you deliver content\. 

In "[For Impatient Web Users, an Eye Blink Is Just Too Long to Wait](http://www.nytimes.com/2012/03/01/technology/impatient-web-users-flee-slow-loading-sites.html?pagewanted=all&_r=0)," the New York Times noted that users can register a 250\-millisecond \(1/4 second\) difference between competing sites\. Users tend to opt out of the slower site in favor of the faster site\. Tests done at Amazon, cited in [How Webpage Load Time Is Related to Visitor Loss](http://pearanalytics.com/blog/2009/how-webpage-load-time-related-to-visitor-loss/), revealed that for every 100\-ms \(1/10 second\) increase in load time, sales decrease 1 percent\. 

If someone wants data, you can deliver that data much faster if it's cached\. That's true whether it's for a webpage or a report that drives business decisions\. Can your business afford to not cache your webpages so as to deliver them with the shortest latency possible?

It might seem intuitively obvious that you want to cache your most heavily requested items\. But why not cache your less frequently requested items? Even the most optimized database query or remote API call is noticeably slower than retrieving a flat key from an in\-memory cache\. *Noticeably slower* tends to send customers elsewhere\.

The following examples illustrate some of the ways using ElastiCache can improve overall performance of your application\.

## In\-Memory Data Store<a name="elasticache-use-cases-data-store"></a>

The primary purpose of an in\-memory key\-value store is to provide ultrafast \(submillisecond latency\) and inexpensive access to copies of data\. Most data stores have areas of data that are frequently accessed but seldom updated\. Additionally, querying a database is always slower and more expensive than locating a key in a key\-value pair cache\. Some database queries are especially expensive to perform\. An example is queries that involve joins across multiple tables or queries with intensive calculations\. By caching such query results, you pay the price of the query only once\. Then you can quickly retrieve the data multiple times without having to re\-execute the query\.

### What Should I Cache?<a name="elasticache-use-cases-data-store-what-to-cache"></a>

When deciding what data to cache, consider these factors:

**Speed and expense** – It's always slower and more expensive to get data from a database than from a cache\. Some database queries are inherently slower and more expensive than others\. For example, queries that perform joins on multiple tables are much slower and more expensive than simple, single table queries\. If the interesting data requires a slow and expensive query to get, it's a candidate for caching\. If getting the data requires a relatively quick and simple query, it might still be a candidate for caching, depending on other factors\.

**Data and access pattern** – Determining what to cache also involves understanding the data itself and its access patterns\. For example, it doesn't make sense to cache data that changes quickly or is seldom accessed\. For caching to provide a real benefit, the data should be relatively static and frequently accessed\. An example is a personal profile on a social media site\. On the other hand, you don't want to cache data if caching it provides no speed or cost advantage\. For example, it doesn't make sense to cache webpages that return search results because the queries and results are usually unique\.

**Staleness** – By definition, cached data is stale data\. Even if in certain circumstances it isn't stale, it should always be considered and treated as stale\. To tell whether your data is a candidate for caching, determine your application's tolerance for stale data\. 

Your application might be able to tolerate stale data in one context, but not another\. For example, suppose that your site serves a publicly traded stock price\. Your customers might accept some staleness with a disclaimer that prices might be *n* minutes delayed\. But if you serve that stock price to a broker making a sale or purchase, you want real\-time data\.

Consider caching your data if the following is true:
+ Your data is slow or expensive to get when compared to cache retrieval\.
+ Users access your data often\.
+ Your data stays relatively the same, or if it changes quickly staleness is not a large issue\.

For more information, see the following:
+ [Caching Strategies](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html) in the *ElastiCache for Memcached User Guide*

## ElastiCache Customer Testimonials<a name="elasticache-use-cases-testimonials"></a>

To learn about how businesses like Airbnb, PBS, Esri, and others use Amazon ElastiCache to grow their businesses with improved customer experience, see [How Others Use Amazon ElastiCache](https://aws.amazon.com/elasticache/testimonials/)\.

You can also watch the [ElastiCache Videos](Tutorials.md#tutorial-videos) for additional ElastiCache customer use cases\.