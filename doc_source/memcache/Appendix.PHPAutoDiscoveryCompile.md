# Compiling the Source Code for the ElastiCache Cluster Client for PHP<a name="Appendix.PHPAutoDiscoveryCompile"></a>

This section covers how to obtain and compile the source code for the ElastiCache Cluster Client for PHP\.

There are two packages you need to pull from GitHub and compile; [aws\-elasticache\-cluster\-client\-libmemcached](https://github.com/awslabs/aws-elasticache-cluster-client-libmemcached) and [aws\-elasticache\-cluster\-client\-memcached\-for\-php](https://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php)\.

**Topics**
+ [Compiling the libmemcached Library](#Appendix.PHPAutoDiscoveryCompile.Libmemcached)
+ [Compiling the ElastiCache Memcached Auto Discovery Client for PHP](#Appendix.PHPAutoDiscoveryCompile.Client)

## Compiling the libmemcached Library<a name="Appendix.PHPAutoDiscoveryCompile.Libmemcached"></a>

**To compile the aws\-elasticache\-cluster\-client\-libmemcached library**

1. Launch an Amazon EC2 instance\.

1. Install the library dependencies\.
   + On Amazon Linux 201509 AMI

     ```
     sudo yum install gcc gcc-c++ autoconf libevent-devel
     ```
   + On Ubuntu 14\.04 AMI

     ```
     sudo apt-get update
     sudo apt-get install libevent-dev gcc g++ make autoconf libsasl2-dev
     ```

1. Pull the repository and compile the code\.

   ```
   Download and expand [https://github.com/awslabs/aws-elasticache-cluster-client-libmemcached/archive/v1.0.18.tar.gz](https://github.com/awslabs/aws-elasticache-cluster-client-libmemcached/archive/v1.0.18.tar.gz)
   cd aws-elasticache-cluster-client-libmemcached
   mkdir BUILD
   cd BUILD
   ../configure --prefix=<libmemcached-install-directory> --with-pic
   make
   sudo make install
   ```

## Compiling the ElastiCache Memcached Auto Discovery Client for PHP<a name="Appendix.PHPAutoDiscoveryCompile.Client"></a>

The following sections describe how to compile the ElastiCache Memcached Auto Discovery Client

**Topics**
+ [Compiling the ElastiCache Memcached Client for PHP 7](#Appendix.PHPAudiscoveryCompile.Client.PHP7)
+ [Compiling the ElastiCache Memcached Client for PHP 5](#Appendix.PHPAudiscoveryCompile.PHP5)

### Compiling the ElastiCache Memcached Client for PHP 7<a name="Appendix.PHPAudiscoveryCompile.Client.PHP7"></a>

Run the following set of commands under the code directory\.

```
git clone  https://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php/tree/php7.git
cd aws-elasticache-cluster-client-memcached-for-php 
git checkout php7
sudo yum install php70-devel
phpize
./configure --with-libmemcached-dir=<libmemcached-install-directory> --disable-memcached-sasl
make
make install
```

**Note**  
You can statically link the libmemcached library into the PHP binary so it can be ported across various Linux platforms\. To do that, run the following command before `make`:  

```
sed -i "s#-lmemcached#<libmemcached-install-directory>/lib/libmemcached.a -lcrypt -lpthread -lm -lstdc++ -lsasl2#" Makefile 
```

### Compiling the ElastiCache Memcached Client for PHP 5<a name="Appendix.PHPAudiscoveryCompile.PHP5"></a>

Compile the `aws-elasticache-cluster-client-memcached-for-php` by running the following commands under the `aws-elasticache-cluster-client-memcached-for-php/` folder\.

```
git clone  https://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php/tree/php7
cd aws-elasticache-cluster-client-memcached-for-php 
sudo yum install zlib-devel
phpize
./configure --with-libmemcached-dir=<libmemcached-install-directory>
make
make install
```