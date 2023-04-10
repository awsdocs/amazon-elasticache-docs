# Using the ElastiCache Cluster Client for PHP<a name="AutoDiscovery.Using.ModifyApp.PHP"></a>

The program below demonstrates how to use the ElastiCache Cluster Client to connect to a cluster configuration endpoint and add a data item to the cache\. Using Auto Discovery, the program will connect to all of the nodes in the cluster without any further intervention\.

To use the ElastiCache Cluster Client for PHP, you will first need to install it on your Amazon EC2 instance\. For more information, see [Installing the ElastiCache cluster client for PHP](Appendix.PHPAutoDiscoverySetup.md)

```
<?php
	
 /**
  * Sample PHP code to show how to integrate with the Amazon ElastiCache
  * Auto Discovery feature.
  */

  /* Configuration endpoint to use to initialize memcached client. 
   * This is only an example. 	*/
  $server_endpoint = "mycluster.fnjyzo.cfg.use1.cache.amazonaws.com";
  
  /* Port for connecting to the ElastiCache cluster. 
   * This is only an example 	*/
  $server_port = 11211;

 /**
  * The following will initialize a Memcached client to utilize the Auto Discovery feature.
  * 
  * By configuring the client with the Dynamic client mode with single endpoint, the
  * client will periodically use the configuration endpoint to retrieve the current cache
  * cluster configuration. This allows scaling the cache cluster up or down in number of nodes
  * without requiring any changes to the PHP application. 
  *
  * By default the Memcached instances are destroyed at the end of the request. 
  * To create an instance that persists between requests, 
  *    use persistent_id to specify a unique ID for the instance. 
  * All instances created with the same persistent_id will share the same connection. 
  * See [http://php\.net/manual/en/memcached\.construct\.php](http://php.net/manual/en/memcached.construct.php) for more information.
  */
  $dynamic_client = new Memcached('persistent-id');
  $dynamic_client->setOption(Memcached::OPT_CLIENT_MODE, Memcached::DYNAMIC_CLIENT_MODE);
  $dynamic_client->addServer($server_endpoint, $server_port);
  
  /**
  * Store the data for 60 seconds in the cluster. 
  * The client will decide which cache host will store this item.
  */  
  $dynamic_client->set('key', 'value', 60);  


 /**
  * Configuring the client with Static client mode disables the usage of Auto Discovery
  * and the client operates as it did before the introduction of Auto Discovery. 
  * The user can then add a list of server endpoints.
  */
  $static_client = new Memcached('persistent-id');
  $static_client->setOption(Memcached::OPT_CLIENT_MODE, Memcached::STATIC_CLIENT_MODE);
  $static_client->addServer($server_endpoint, $server_port);

 /**
  * Store the data without expiration. 
  * The client will decide which cache host will store this item.
  */  
  $static_client->set('key', 'value');  
  ?>
```

For an example on how to use the ElastiCache Cluster Client with TLS enabled, see [Creating a TLS Memcached client using PHP](in-transit-encryption-mc.md#in-transit-encryption-connect-php)\.