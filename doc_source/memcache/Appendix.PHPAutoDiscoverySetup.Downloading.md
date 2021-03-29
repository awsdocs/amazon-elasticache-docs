# Downloading the installation package<a name="Appendix.PHPAutoDiscoverySetup.Downloading"></a>

To ensure that you use the correct version of the ElastiCache Cluster Client for PHP, you will need to know what version of PHP is installed on your Amazon EC2 instance\. You will also need to know whether your Amazon EC2 instance is running a 64\-bit or 32\-bit version of Linux\.

**To determine the PHP version installed on your Amazon EC2 instance**
+ At the command prompt, run the following command:

  ```
  php -v
  ```

  The PHP version will be shown in the output, as in this example:

  ```
  PHP 5.4.10 (cli) (built: Jan 11 2013 14:48:57) 
  Copyright (c) 1997-2012 The PHP Group
  Zend Engine v2.4.0, Copyright (c) 1998-2012 Zend Technologies
  ```
**Note**  
If your PHP and Memcached versions are incompatible, you will get an error message something like the following:  

  ```
  PHP Warning: PHP Startup: memcached: Unable to initialize module
  Module compiled with module API=20100525
  PHP compiled with module API=20131226
  These options need to match
  in Unknown on line 0
  ```
If this happens, you need to compile the module from the source code\. For more information, see [Compiling the source code for the ElastiCache cluster client for PHP](Appendix.PHPAutoDiscoveryCompile.md)\.

**To determine your Amazon EC2 AMI architecture \(64\-bit or 32\-bit\)**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the **Instances** list, click your Amazon EC2 instance\.

1. In the **Description** tab, look for the **AMI:** field\. A 64\-bit instance should have `x86_64` as part of the description; for a 32\-bit instance, look for `i386` or `i686` in this field\.

You are now ready to download the ElastiCache Cluster Client\.

**To download the ElastiCache cluster client for PHP**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the ElastiCache console, choose **ElastiCache Cluster Client**\.

1. From the **Download ElastiCache Memcached Cluster Client** list, choose the ElastiCache Cluster Client that matches your PHP version and AMI architecture, then choose the **Download** button\.