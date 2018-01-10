# Removing the PHP Cluster Client<a name="Appendix.PHPAutoDiscoverySetup.Removing"></a>


+ [Removing an earlier version of PHP 7](#Appendix.PHPAutoDiscoverySetup.Removing.PHP7x)
+ [Removing an earlier version of PHP 5](#Appendix.PHPAutoDiscoverySetup.Removing.PHP5x)

## Removing an earlier version of PHP 7<a name="Appendix.PHPAutoDiscoverySetup.Removing.PHP7x"></a>

**To remove an earlier version of PHP 7**

1. Remove the `amazon-elasticache-cluster-client.so` file from the appropriate PHP lib directory as previously indicated in the installation instructions\. See the section for your installation at [For Users Who Already Have *php\-memcached* Extension Installed](Appendix.PHPAutoDiscoverySetup.md#Appendix.PHPAutoDiscoverySetup.InstallingExisting)\.

1. Remove the line `extension=amazon-elasticache-cluster-client.so` from the `php.ini` file\.

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

## Removing an earlier version of PHP 5<a name="Appendix.PHPAutoDiscoverySetup.Removing.PHP5x"></a>

**To remove an earlier version of PHP 5**

1. Remove the `php-memcached` extension:

   ```
   sudo pecl uninstall __uri/AmazonElastiCacheClusterClient
   ```

1.  Remove the `memcached.ini` file added in the appropriate directory as indicated in the previous installation steps\. 