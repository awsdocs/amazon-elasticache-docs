# Installing the ElastiCache Cluster Client for PHP<a name="Appendix.PHPAutoDiscoverySetup"></a>

This section describes how to install, update, and remove the PHP components for the ElastiCache Cluster Client on Amazon EC2 instances\. For more information about Auto Discovery, see [Automatically Identify Nodes in your Memcached Cluster](AutoDiscovery.md)\. For sample PHP code to use the client\. see [Using the ElastiCache Cluster Client for PHP](AutoDiscovery.Using.md#AutoDiscovery.Using.ModifyApp.PHP)\.

**Topics**
+ [Downloading the Installation Package](Appendix.PHPAutoDiscoverySetup.Downloading.md)
+ [For Users Who Already Have *php\-memcached* Extension Installed](#Appendix.PHPAutoDiscoverySetup.InstallingExisting)
+ [Installation Steps for New Users](Appendix.PHPAutoDiscoverySetup.Installing.md)
+ [Removing the PHP Cluster Client](Appendix.PHPAutoDiscoverySetup.Removing.md)

## For Users Who Already Have *php\-memcached* Extension Installed<a name="Appendix.PHPAutoDiscoverySetup.InstallingExisting"></a>

**To update the `php-memcached` installation**

1. Remove the previous installation of the Memcached extension for PHP as described by the topic [Removing the PHP Cluster Client](Appendix.PHPAutoDiscoverySetup.Removing.md)\.

1. Install the new ElastiCache `php-memcached` extension as described previously in [Installation Steps for New Users](Appendix.PHPAutoDiscoverySetup.Installing.md)\. 