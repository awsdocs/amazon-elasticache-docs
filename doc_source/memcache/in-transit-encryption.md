# ElastiCache in\-transit encryption \(TLS\)<a name="in-transit-encryption"></a>

To help keep your data secure, Amazon ElastiCache and Amazon EC2 provide mechanisms to guard against unauthorized access of your data on the server\. By providing in\-transit encryption capability, ElastiCache gives you a tool you can use to help protect your data when it is moving from one location to another\. 

The parameters `TransitEncryptionEnabled` \(CLI: `--transit-encryption-enabled`\) are only available when using the `CreateCacheCluster` \(CLI: `create-cache-cluster`\) operation\.

**Topics**
+ [In\-transit encryption overview](#in-transit-encryption-overview)
+ [In\-transit encryption conditions](#in-transit-encryption-constraints)
+ [In\-transit encryption best practices](#in-transit-encryption-best-practices)
+ [Enabling in\-transit encryption](#in-transit-encryption-enable-existing)
+ [Connecting to nodes enabled with in\-transit encryption using Openssl](#in-transit-encryption-connect)
+ [Creating a TLS Memcached client using Java](#in-transit-encryption-connect-java)
+ [Creating a TLS Memcached client using PHP](#in-transit-encryption-connect-php)

## In\-transit encryption overview<a name="in-transit-encryption-overview"></a>

Amazon ElastiCache in\-transit encryption is an optional feature that allows you to increase the security of your data at its most vulnerable points—when it is in transit from one location to another\. Because there is some processing needed to encrypt and decrypt the data at the endpoints, enabling in\-transit encryption can have some performance impact\. You should benchmark your data with and without in\-transit encryption to determine the performance impact for your use cases\.

ElastiCache in\-transit encryption implements the following features:
+ **Encrypted connections**—both the server and client connections are Secure Socket Layer \(SSL\) encrypted\.
+ **Server authentication**—clients can authenticate that they are connecting to the right server\.

## In\-transit encryption conditions<a name="in-transit-encryption-constraints"></a>

The following constraints on Amazon ElastiCache in\-transit encryption should be kept in mind when you plan your implementation:
+ In\-transit encryption is supported on clusters running Memcached versions 1\.6\.12 and later\.
+ In\-transit encryption supports TLS versions 1\.2 and 1\.3\.
+ In\-transit encryption is supported only for clusters running in an Amazon VPC\.
+ In\-transit encryption is only supported for clusters running the following node types\.
  + R6g, R5, R4
  + M6g, M5, M4
  + T4g, T3

  For more information, see [Supported node types](CacheNodes.SupportedTypes.md)\.
+ In\-transit encryption is enabled by explicitly setting the parameter `TransitEncryptionEnabled` to `true`\.
+ You can enable in\-transit encryption on a cluster only when creating the cluster\. You cannot toggle in\-transit encryption on and off by modifying a cluster\. 
+ To connect to an in\-transit encryption enabled cluster, it must be enabled for transport layer security \(TLS\)\. To connect to a cluster that is not in\-transit encryption enabled, the cluster cannot be TLS\-enabled\.

## In\-transit encryption best practices<a name="in-transit-encryption-best-practices"></a>
+ Because of the processing required to encrypt and decrypt the data at the endpoints, implementing in\-transit encryption can reduce performance\. Benchmark in\-transit encryption compared to no encryption on your own data to determine its impact on performance for your implementation\.
+ Because creating new connections can be expensive, you can reduce the performance impact of in\-transit encryption by persisting your TLS connections\.

## Enabling in\-transit encryption<a name="in-transit-encryption-enable-existing"></a>

To enable in\-transit encryption when creating a Memcached cluster using the AWS Management Console, make the following selections:
+ Choose Memcached as your engine\.
+ Choose engine version 1\.6\.12 or later\.
+ Under **Encryption in transit**, choose **Enable**\.

 For the step\-by\-step process, see [Creating a Memcached cluster \(console\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Clusters.Create.html)\. 

## Connecting to nodes enabled with in\-transit encryption using Openssl<a name="in-transit-encryption-connect"></a>

To access data from ElastiCache for Memcached nodes enabled with in\-transit encryption, you need to use clients that work with Secure Socket Layer \(SSL\)\. You can also use Openssl s\_client on Amazon Linux and Amazon Linux 2\. 

To use Openssl s\_client to connect to a Memcached cluster enabled with in\-transit encryption on Amazon Linux 2 or Amazon Linux:

```
/usr/bin/openssl s_client -connect memcached-node-endpoint:memcached-port
```

## Creating a TLS Memcached client using Java<a name="in-transit-encryption-connect-java"></a>

To create a client in TLS mode, do the following to initialize the client with the appropriate SSLContext:

```
import java.security.KeyStore;
import javax.net.ssl.SSLContext;
import javax.net.ssl.TrustManagerFactory;
import net.spy.memcached.AddrUtil;
import net.spy.memcached.ConnectionFactoryBuilder;
import net.spy.memcached.MemcachedClient;
public class TLSDemo {
    public static void main(String[] args) throws Exception {
        ConnectionFactoryBuilder connectionFactoryBuilder = new ConnectionFactoryBuilder();
        // Build SSLContext
        TrustManagerFactory tmf = TrustManagerFactory.getInstance(TrustManagerFactory.getDefaultAlgorithm());
        tmf.init((KeyStore) null);
        SSLContext sslContext = SSLContext.getInstance("TLS");
        sslContext.init(null, tmf.getTrustManagers(), null);
        // Create the client in TLS mode
        connectionFactoryBuilder.setSSLContext(sslContext);
        MemcachedClient client = new MemcachedClient(connectionFactoryBuilder.build(), AddrUtil.getAddresses("mycluster.fnjyzo.cfg.use1.cache.amazonaws.com:11211"));

        // Store a data item for an hour.
        client.set("theKey", 3600, "This is the data value");
    }
}
```

For more information on using the Java client, see [Using the ElastiCache Cluster Client for Java](AutoDiscovery.Using.ModifyApp.Java.md)\.

## Creating a TLS Memcached client using PHP<a name="in-transit-encryption-connect-php"></a>

To create a client in TLS mode, do the following to initialize the client with the appropriate SSLContext:

See [Using the ElastiCache Cluster Client for PHP](AutoDiscovery.Using.ModifyApp.PHP.md) for more information about Auto Discovery and persistent\-id\.

```
<?php

/**
 * Sample PHP code to show how to create a TLS Memcached client. In this example we
 * will use the Amazon ElastiCache Auto Descovery feature, but TLS can also be
 * used with a Static mode client. 
 * See Using the ElastiCache Cluster Client for PHP (https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/AutoDiscovery.Using.ModifyApp.PHP.html) for more information 
 * about Auto Discovery and persistent-id.
 */

/* Configuration endpoint to use to initialize memcached client.
 * this is only an example */
$server_endpoint = "mycluster.fnjyzo.cfg.use1.cache.amazonaws.com";

/* Port for connecting to the cluster. 
 * This is only an example     */
$server_port = 11211;

/* Initialize a persistent Memcached client and configure it with the Dynamic client mode  */
$tls_client =  new Memcached('persistent-id');
$tls_client->setOption(Memcached::OPT_CLIENT_MODE, Memcached::DYNAMIC_CLIENT_MODE);

/* Add the memcached's cluster server/s */
$tls_client->addServer($server_endpoint, $server_port);

/* Configure the client to use TLS */
if(!$tls_client->setOption(Memcached::OPT_USE_TLS, 1)) {
    echo $tls_client->getLastErrorMessage(), "\n";
    exit(1);
}

/* Set your TLS context configurations values.
 * See MemcachedTLSContextConfig in memcached-api.php for all configurations */
$tls_config = new MemcachedTLSContextConfig();
$tls_config->cert_file = '/path/to/memc.crt';
$tls_config->key_file = '/path/to/memc.key';
$tls_config->ca_cert_file = '/path/to/ca.crt';
$tls_config->hostname = '*.mycluster.fnjyzo.use1.cache.amazonaws.com';
$tls_config->skip_cert_verify = false;
$tls_config->skip_hostname_verify = false;

/* Use the created TLS context configuration object to create OpenSSL's SSL_CTX and set it to your client.
 * Note:  These TLS context configurations will be applied to all the servers connected to this client. */
$tls_client->createAndSetTLSContext((array)$tls_config);

/* test the TLS connection with set-get scenario: */

 /* store the data for 60 seconds in the cluster.
 * The client will decide which cache host will store this item.
 */
if($tls_client->set('key', 'value', 60)) {
    print "Successfully stored key\n";
} else {
    echo "Failed to set key: ", $tls_client->getLastErrorMessage(), "\n";
    exit(1);
}

/* retrieve the key */
if ($tls_client->get('key') === 'value') {
    print "Successfully retrieved key\n";
} else {
    echo "Failed to get key: ", $tls_client->getLastErrorMessage(), "\n";
    exit(1);
}
```

For more information on using the PHP client, see [Installing the ElastiCache cluster client for PHP](Appendix.PHPAutoDiscoverySetup.md)\.