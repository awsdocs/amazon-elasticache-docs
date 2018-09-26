# Use Cases and How ElastiCache Can Help<a name="elasticache-use-cases"></a>

Whether serving the latest news or a product catalog, or selling tickets to an event, speed is the name of the game\. The success of your website and business is significantly affected by the speed at which you deliver content\. According to the New York Times in 2012, in "[For Impatient Web Users, an Eye Blink Is Just Too Long to Wait](http://www.nytimes.com/2012/03/01/technology/impatient-web-users-flee-slow-loading-sites.html?pagewanted=all&_r=0)," users can register a 250\-millisecond \(1/4 second\) difference between competing sites\. They tend to opt out of the slower site in favor of the faster site\. Tests done at Amazon in 2007, cited in [How Webpage Load Time Is Related to Visitor Loss](http://pearanalytics.com/blog/2009/how-webpage-load-time-related-to-visitor-loss/), revealed that for every 100\-ms \(1/10 second\) increase in load time, sales decrease 1 percent\. If someone wants data, whether for a webpage or a report that drives business decisions, you can deliver that data much faster if it's cached\. Can your business afford to not cache your webpages so as to deliver them with the shortest latency possible?

It might be intuitively obvious that you want to cache your most heavily requested items\. But why not cache your less frequently requested items? Even the most optimized database query or remote API call is going to be noticeably slower than retrieving a flat key from an in\-memory cache\. *Noticeably slower* tends to send customers elsewhere\.

The following examples illustrate some of the ways using ElastiCache can improve overall performance of your application\.

## In\-Memory Data Store<a name="elasticache-use-cases-data-store"></a>

The primary purpose of an in\-memory key\-value store is to provide ultrafast \(submillisecond latency\) and inexpensive access to copies of data\. Most data stores have areas of data that are frequently accessed but seldom updated\. Additionally, querying a database is always slower and more expensive than locating a key in a key\-value pair cache\. Some database queries are especially expensive to perform, for example, queries that involve joins across multiple tables or queries with intensive calculations\. By caching such query results, you pay the price of the query once and then are able to quickly retrieve the data multiple times without having to re\-execute the query\.

The following image shows ElastiCache caching\.

![\[Image: ElastiCache Claching\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/./images/ElastiCache-Caching.png)

### What Should I Cache?<a name="elasticache-use-cases-data-store-what-to-cache"></a>

When deciding what data to cache, consider these factors:

**Speed and Expense** – It's always slower and more expensive to acquire data from a database than from a cache\. Some database queries are inherently slower and more expensive than others\. For example, queries that perform joins on multiple tables are significantly slower and more expensive than simple, single table queries\. If the interesting data requires a slow and expensive query to acquire, it's a candidate for caching\. If acquiring the data requires a relatively quick and simple query, it might still be a candidate for caching, depending on other factors\.

**Data and Access Pattern** – Determining what to cache also involves understanding the data itself and its access patterns\. For example, it doesn't make sense to cache data that is rapidly changing or is seldom accessed\. For caching to provide a meaningful benefit, the data should be relatively static and frequently accessed, such as a personal profile on a social media site\. Conversely, you don't want to cache data if caching it provides no speed or cost advantage\. For example, it doesn't make sense to cache webpages that return the results of a search because such queries and results are almost always unique\.

**Staleness** – By definition, cached data is stale data—even if in certain circumstances it isn't stale, it should always be considered and treated as stale\. In determining whether your data is a candidate for caching, you need to determine your application's tolerance for stale data\. Your application might be able to tolerate stale data in one context, but not another\. For example, when serving a publicly traded stock price on a website, staleness might be acceptable, with a disclaimer that prices might be up to *n* minutes delayed\. But when serving up the price for the same stock to a broker making a sale or purchase, you want real\-time data\.

In summary, consider caching your data if the following is true:
+ It is slow or expensive to acquire when compared to cache retrieval\.
+ It is accessed with sufficient frequency\.
+ It is relatively static, or if rapidly changing, staleness is not a significant issue\.

For more information, see the following:
+ [https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html) in the ElastiCache for Memcached User Guide

## ElastiCache Customer Testimonials<a name="elasticache-use-cases-testimonials"></a>

To learn about how businesses like Airbnb, PBS, Esri, and others are using Amazon ElastiCache to grow their businesses with improved customer experience, see [How Others Use Amazon ElastiCache](https://aws.amazon.com/elasticache/testimonials/)\.

You can also watch the [ElastiCache Videos](Tutorials.md#tutorial-videos) for additional ElastiCache customer use cases\.