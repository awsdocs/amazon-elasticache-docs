# Compiling the source code for the ElastiCache cluster client for PHP<a name="Appendix.PHPAutoDiscoveryCompile"></a>

This section covers how to obtain and compile the source code for the ElastiCache Cluster Client for PHP\.

There are two packages you need to pull from GitHub and compile; [aws\-elasticache\-cluster\-client\-libmemcached](https://github.com/awslabs/aws-elasticache-cluster-client-libmemcached) and [aws\-elasticache\-cluster\-client\-memcached\-for\-php](https://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php)\.

**Topics**
+ [Compiling the libmemcached library](#Appendix.PHPAutoDiscoveryCompile.Libmemcached)
+ [Compiling the ElastiCache Memcached auto discovery client for PHP](#Appendix.PHPAutoDiscoveryCompile.Client)

## Compiling the libmemcached library<a name="Appendix.PHPAutoDiscoveryCompile.Libmemcached"></a>

**Prerequesite libraries**
+ OpenSSL 1\.1\.0 or higher \(unless TLS support is disabled by \./configure \-\-disable\-tls\)\.
+ SASL \(libsasl2, unless SASL support is disabled by `./configure --disable-sasl`\)\.

**To compile the aws\-elasticache\-cluster\-client\-libmemcached library**

1. Launch an Amazon EC2 instance\.

1. Install the library dependencies\.
   + On Amazon Linux 201509 AMI / Amazon Linux 2 AMI

     ```
     sudo yum -y update
     sudo yum install gcc gcc-c++ autoconf libevent-devel make perl-core pcre-devel wget zlib-devel 
     // Install OpenSSL 1.1.1
     wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz
     tar xvf openssl-1.1.1c.tar.gz
     cd openssl-1.1.1c
     ./config
     make
     sudo make install
     sudo ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/libssl.so.1.1
     ```
   + On Ubuntu 14\.04 AMI \(not required for Ubuntu versions that come with OpenSSL >= 1\.1\)

     ```
     sudo apt-get update
     
     sudo apt-get install libevent-dev gcc g++ make autoconf libsasl2-dev
     
     // Install OpenSSL 1.1.1
     wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz
     tar xvf openssl-1.1.1c.tar.gz
     cd openssl-1.1.1c
     ./config
     make
     sudo make install
     sudo ln -s /usr/local/lib/libssl.so.1.1 /usr/lib/x86_64-linux-gnu/libssl.so.1.1
     ```

1. Pull the repository and compile the code\.

   ```
   git clone https://github.com/awslabs/aws-elasticache-cluster-client-libmemcached.git
   cd aws-elasticache-cluster-client-libmemcached
   touch configure.ac aclocal.m4 configure Makefile.am Makefile.in
   mkdir BUILD
   cd BUILD
   ../configure --prefix=<libmemcached-install-directory> --with-pic --disable-sasl
   ```

   If running `../configure` fails to find `libssl` \(OpenSSL library\) it may be necessary to tweak the `PKG_CONFIG_PATH` environment variable:

   ```
   PKG_CONFIG_PATH=/path/to/ssl/lib/pkgconfig ../configure --prefix=<libmemcached-install-directory> --with-pic --disable-sasl
   ```

   Alternately, if you are not using TLS, you can disable it by running:

   ```
   make
   sudo make install
   ../configure —prefix=<libmemcached-install-directory> --with-pic --disable-sasl --disable-tls
   ```

## Compiling the ElastiCache Memcached auto discovery client for PHP<a name="Appendix.PHPAutoDiscoveryCompile.Client"></a>

The following sections describe how to compile the ElastiCache Memcached Auto Discovery Client

**Topics**
+ [Compiling the ElastiCache Memcached client for PHP 7 or higher](#Appendix.PHPAudiscoveryCompile.Client.PHP7)
+ [Compiling the ElastiCache Memcached client for PHP 5](#Appendix.PHPAudiscoveryCompile.PHP5)

### Compiling the ElastiCache Memcached client for PHP 7 or higher<a name="Appendix.PHPAudiscoveryCompile.Client.PHP7"></a>

 Replace PHP\-7\.x with the version you are using\. 

Install PHP:

```
sudo yum install -y amazon-linux-extras
sudo amazon-linux-extras enable php7.x
```

Run the following set of commands under the code directory\.

```
git clone  https://github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php.git
cd aws-elasticache-cluster-client-memcached-for-php 
phpize
mkdir BUILD
CD BUILD
../configure --with-libmemcached-dir=<libmemcached-install-directory> --disable-memcached-sasl
```

If running \.\./configure fails to find libssl \(OpenSSL library\) it may be necessary to tweak the `PKG_CONFIG_PATH ` environment variable to OpenSSL’s \.PC file directory:

```
PKG_CONFIG_PATH=/path/to/ssl/lib/pkgconfig ../configure --with-libmemcached-dir=<path to libmemcached build directory> --disable-memcached-sasl
```

Alternatively, if you are not using TLS, you can disable it by running:

```
make
make install
../configure --with-libmemcached-dir=<path to libmemcached build directory> --disable-memcached-sasl --disable-memcached-tls
```

**Note**  
You can statically link the libmemcached library into the PHP binary so it can be ported across various Linux platforms\. To do that, run the following command before `make`:  

```
sed -i "s#-lmemcached#<libmemcached-install-directory>/lib/libmemcached.a -lcrypt -lpthread -lm -lstdc++ -lsasl2#" Makefile 
```

### Compiling the ElastiCache Memcached client for PHP 5<a name="Appendix.PHPAudiscoveryCompile.PHP5"></a>

Compile the `aws-elasticache-cluster-client-memcached-for-php` by running the following commands under the `aws-elasticache-cluster-client-memcached-for-php/` folder\.

```
git clone  https:////github.com/awslabs/aws-elasticache-cluster-client-memcached-for-php/tree/php.git
cd aws-elasticache-cluster-client-memcached-for-php 
sudo yum install zlib-devel
phpize
./configure --with-libmemcached-dir=<libmemcached-install-directory>
make
make install
```