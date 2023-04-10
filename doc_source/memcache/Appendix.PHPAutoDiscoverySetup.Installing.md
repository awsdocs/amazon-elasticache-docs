# Installation steps for new users<a name="Appendix.PHPAutoDiscoverySetup.Installing"></a>

**Topics**
+ [Installing PHP 7\.x \- 8\.x for new users](#Appendix.PHPAutoDiscoverySetup.Installing.PHP7x)
+ [Installing PHP 5\.x for new users](#Appendix.PHPAutoDiscoverySetup.Installing.PHP5x)

## Installing PHP 7\.x \- 8\.x for new users<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP7x"></a>

**Topics**
+ [To install PHP 7\.x \- 8\.x on an Amazon Linux 2 AMI](#Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.Ubuntu)
+ [To install PHP 7\.x \- 8\.x on an Amazon Linux 201609 AMI](#Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.AmznLinux)
+ [To install PHP 7\.x \- 8\.x on an SUSE Linux 15 AMI](#Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.SuseLinux)
+ [To install PHP 7\.x \- 8\.x on an Ubuntu 22\.04 AMI](#Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.UbuntuLinux22)

### To install PHP 7\.x \- 8\.x on an Amazon Linux 2 AMI<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.Ubuntu"></a>

**Note**  
If necessary, replace *PHP\-7\.x* with the version you are using\.

1. Launch a new instance from the AMI\.

1. Run the following command:

   ```
   sudo yum install gcc-c++ zlib-devel
   ```

1. Install PHP 7\.x using `amazon-linux-extras`

   With Amazon Linux 2, you can use the *Extras Library* to install application and software updates on your instances\. These software updates are known as topics\. You can install a specific version of a topic or omit the version information to use the most recent version\. For more information, [Extras library \(Amazon Linux 2\)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/amazon-linux-ami-basics.html#extras-library)\.

   To do this, use the following steps;

   1. First, verify if *amazon\-linux\-extras* is already installed\.

   1. If not installed, use the following command to install:

       `sudo yum install -y amazon-linux-extras ` 

   1. Confirm that PHP 7\.x topic is available in the Amazon Linux 2 machine:

       `sudo amazon-linux-extras | grep php ` 

   1. From the output, review all PHP 7 topics and select the version you want:

       `sudo amazon-linux-extras enable php7.x ` 

   1. Install PHP packages from the repository\. For example:

       `sudo yum clean metadata`

      `sudo yum install php php-devel ` 

1. Download the Amazon ElastiCache Cluster Client\.
   + Open the ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

     Under the ElastiCache dashboard, go to **ElastiCache Cluster Client** and then choose the PHP7 version you want\.
   + From command line, replace `PHP-7.X` with the desired PHP version, and replace the `ARCH` with desired architecture \(X86 or arm\) and for PHP >= 7\.4 replace the OpenSSL with the desired OpenSSL version \(openssl1\.1 or openssl3\)\. If you are using PHP > 7\.4, remove OpenSSL suffix\.

     ```
     wget https://elasticache-downloads.s3.amazonaws.com/ClusterClient/PHP-7.X/latest-64bit-<ARCH>-<OpenSSL>
     ```

1. Use `tar -zxvf` to extract the downloaded file\.

   ```
   tar -zxvf latest-64bit-<ARCH>-<OpenSSL>
   ```

1. With root permissions, copy the extracted artifact file `amazon-elasticache-cluster-client.so` into `/usr/lib64/php/modules`\.

   ```
   sudo mv amazon-elasticache-cluster-client.so /usr/lib64/php/modules/
   ```

1. Add `extension=amazon-elasticache-cluster-client.so` into file `/etc/php.ini`

1. If you downloaded ElastiCache Cluster Client with PHP 7\.4 or higher, install OpenSSL 1\.1\.x or higher\. Installation instructions for OpenSSL 1\.1\.1:

   ```
   sudo yum -y update
   sudo yum install -y make gcc perl-core pcre-devel wget zlib-devel
   wget https://www.openssl.org/source/openssl-1.1.1c.tar.gz
   tar xvf openssl-1.1.1c.tar.gz
   cd openssl-1.1.1c
   ./config 
   make
   sudo make install
   sudo ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/libssl.so.1.1
   ```

 

### To install PHP 7\.x \- 8\.x on an Amazon Linux 201609 AMI<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.AmznLinux"></a>

**Note**  
If necessary, replace *php7\.x* with the version you are using\.

1. Launch a new instance from the AMI\. For more information, see [Step 1: Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html#ec2-launch-instance) in the *Amazon EC2 User Guide\.*

1. Run the following command:

   ```
   sudo yum install gcc-c++
   ```

1. Install PHP

   ```
   sudo yum install php7.x
   ```

1. Download the Amazon ElastiCache Cluster Client\.

   ```
   wget https://elasticache-downloads.s3.amazonaws.com/ClusterClient/PHP-7.x/latest-64bit
   ```

1. Extract `latest-64bit`\.

   ```
   tar -zxvf latest-64bit
   ```

1. With root permission, copy the extracted artifact file `amazon-elasticache-cluster-client.so` into `/usr/lib64/php/7.x/modules/`\.

   ```
   sudo mv artifact/amazon-elasticache-cluster-client.so /usr/lib64/php/7.x/modules/
   ```

1. Create the `50-memcached.ini` file\.

   ```
   echo "extension=amazon-elasticache-cluster-client.so" | sudo tee --append /etc/php-7.x.d/50-memcached.ini
   ```

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

 

### To install PHP 7\.x \- 8\.x on an SUSE Linux 15 AMI<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.SuseLinux"></a>

**Note**  
If necessary, replace *php7\.x* with the version you are using\.

1. Launch a new instance from the AMI\.

1. Run the following command:

   ```
   sudo zypper refresh
   sudo zypper update -y
   sudo zypper install gcc
   ```

1. Install PHP

   ```
   sudo yum install php7.x
   ```

   or

   ```
   sudo zypper addrepo //download.opensuse.org/repositories/devel:/languages:/php/openSUSE_Leap_15.3/ php
   ```

1. Download the Amazon ElastiCache Cluster Client, replace <ARCH> with desired architecture \(X86 or arm\)\. SUSE 15 comes with OpenSSL1\.1 built in, so for PHP >= 7\.4 choose the client binary with OpenSSL1\. If you are using PHP < 7\.4, remove OpenSSL suffix\.

   ```
   wget https://elasticache-downloads.s3.amazonaws.com/ClusterClient/PHP-7.x/latest-64bit-<ARCH>-openssl1.1
   ```

1. Extract `latest-64bit`\.

   ```
   tar -zxvf latest-64bit-<ARCH>-openssl1.1
   ```

1. With root permission, copy the extracted artifact file `amazon-elasticache-cluster-client.so` into `/usr/lib64/php7/extensions/`\.

   ```
   sudo mv artifact/amazon-elasticache-cluster-client.so /usr/lib64/php7/extensions/
   ```

1. Insert the line `extension=amazon-elasticache-cluster-client.so` into the file `/etc/php7/cli/php.ini`\.

   ```
   echo "extension=amazon-elasticache-cluster-client.so" | sudo tee --append /etc/php7/cli/php.ini
   ```

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

 

### To install PHP 7\.x \- 8\.x on an Ubuntu 22\.04 AMI<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP7x.UbuntuLinux22"></a>

**Note**  
If necessary, replace *php7\.x* with the version you are using\.

1. Launch a new instance from the AMI\.

1. Run the following command:

   ```
   sudo apt-get update
   sudo apt-get install gcc g++ make zlib1g zlib1g-dev
   ```

1. Install PHP

   1. Installation instructions for PHP 8\.1:

      ```
      sudo apt install php8.1-cli php8.1-dev
      ```

   1. Installation instructions for PHP 7\.4: 

      ```
      sudo apt -y install software-properties-common
      sudo add-apt-repository ppa:ondrej/php
      sudo apt-get update
      sudo apt -y install php7.4
      ```

1. Download the Amazon ElastiCache Cluster Client, replace <ARCH> with desired architecture \(X86 or arm\)\. Ubuntu 22\.04 comes with OpenSSL3 built in, so for PHP >= 7\.4 choose the client binary with OpenSSL3\. If you are using PHP < 7\.4, remove OpenSSL suffix\.

   ```
   wget https://elasticache-downloads.s3.amazonaws.com/ClusterClient/PHP-7.x/latest-64bit-<ARCH>-openssl3
   ```

1. Extract latest\-64bit\.

   ```
   tar -zxvf latest-64bit-<ARCH>-openssl3
   ```

1. With root permission, copy the extracted artifact file `amazon-elasticache-cluster-client.so` into the php extension directory `/usr/lib/php/20190902`\. In case this extension dir does not exist, you can find it by running: `php -i | grep extension_dir`

1. Insert the line `extension=amazon-elasticache-cluster-client.so` into the file `/etc/php/7.x/cli/php.ini`\.

 

## Installing PHP 5\.x for new users<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP5x"></a>

**Topics**
+ [To install PHP 5 on an Amazon Linux AMI 2014\.03 \(64\-bit and 32\-bit\)](#Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.AmznLinux)
+ [To install PHP 5 on a Red Hat Enterprise Linux 7\.0 AMI \(64\-bit and 32\-bit\)](#Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.RHEL)
+ [To install PHP 5 on a Ubuntu server 14\.04 LTS AMI \(64\-bit and 32\-bit\)](#Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.Ubuntu)
+ [To install PHP 5 for SUSE Linux enterprise server 11 AMI \(64\-bit or 32\-bit\)](#Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.SuseLinux)
+ [Other Linux distributions](#Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.Other)

### To install PHP 5 on an Amazon Linux AMI 2014\.03 \(64\-bit and 32\-bit\)<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.AmznLinux"></a>

1. Launch an Amazon Linux instance \(either 64\-bit or 32\-bit\) and log into it\.

1. Install PHP dependencies:

   ```
   $ sudo yum install gcc-c++ php php-pear
   ```

1. Download the correct `php-memcached` package for your Amazon EC2 instance and PHP version\. For more information, see [Downloading the installation package](Appendix.PHPAutoDiscoverySetup.Downloading.md)\.

1. Install `php-memcached`\. The URI should be the download path for the installation package:

   ```
   $ sudo pecl install <package download path>
   ```

   Here is a sample installation command for PHP 5\.4, 64\-bit Linux\. In this sample, replace *X\.Y\.Z* with the actual version number:

   ```
   $ sudo pecl install /home/AmazonElastiCacheClusterClient-X.Y.Z-PHP54-64bit.tgz
   ```
**Note**  
Be sure to use the latest version of the install artifact\.

1. With root/sudo permission, add a new file named `memcached.ini` in the `/etc/php.d` directory, and insert "extension=amazon\-elasticache\-cluster\-client\.so" in the file: 

   ```
   $ echo "extension=amazon-elasticache-cluster-client.so" | sudo tee --append /etc/php.d/memcached.ini
   ```

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

 

### To install PHP 5 on a Red Hat Enterprise Linux 7\.0 AMI \(64\-bit and 32\-bit\)<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.RHEL"></a>

1. Launch a Red Hat Enterprise Linux instance \(either 64\-bit or 32\-bit\) and log into it\.

1. Install PHP dependencies:

   ```
   sudo yum install gcc-c++ php php-pear
   ```

1. Download the correct `php-memcached` package for your Amazon EC2 instance and PHP version\. For more information, see [Downloading the installation package](Appendix.PHPAutoDiscoverySetup.Downloading.md)\.

1. Install `php-memcached`\. The URI should be the download path for the installation package:

   ```
   sudo pecl install <package download path>
   ```

1. With root/sudo permission, add a new file named `memcached.ini` in the `/etc/php.d` directory, and insert `extension=amazon-elasticache-cluster-client.so` in the file\.

   ```
   echo "extension=amazon-elasticache-cluster-client.so" | sudo tee --append /etc/php.d/memcached.ini
   ```

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

 

### To install PHP 5 on a Ubuntu server 14\.04 LTS AMI \(64\-bit and 32\-bit\)<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.Ubuntu"></a>

1. Launch an Ubuntu Linux instance \(either 64\-bit or 32\-bit\) and log into it\.

1. Install PHP dependencies:

   ```
   sudo apt-get update 
   sudo apt-get install gcc g++ php5 php-pear
   ```

1. Download the correct `php-memcached` package for your Amazon EC2 instance and PHP version\. For more information, see [Downloading the installation package](Appendix.PHPAutoDiscoverySetup.Downloading.md)\. 

1. Install `php-memcached`\. The URI should be the download path for the installation package\. 

   ```
   $ sudo pecl install <package download path>
   ```
**Note**  
This installation step installs the build artifact `amazon-elasticache-cluster-client.so` into the `/usr/lib/php5/20121212*` directory\. Verify the absolute path of the build artifact, because you need it in the next step\. 

   If the previous command doesn't work, you need to manually extract the PHP client artifact `amazon-elasticache-cluster-client.so` from the downloaded `*.tgz` file, and copy it to the `/usr/lib/php5/20121212*` directory\.

   ```
   $ tar -xvf <package download path>
   cp amazon-elasticache-cluster-client.so /usr/lib/php5/20121212/
   ```

1. With root/sudo permission, add a new file named `memcached.ini` in the `/etc/php5/cli/conf.d` directory, and insert "extension=<absolute path to amazon\-elasticache\-cluster\-client\.so>" in the file\.

   ```
   $ echo "extension=<absolute path to amazon-elasticache-cluster-client.so>" | sudo tee --append /etc/php5/cli/conf.d/memcached.ini
   ```

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

 

### To install PHP 5 for SUSE Linux enterprise server 11 AMI \(64\-bit or 32\-bit\)<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.SuseLinux"></a>

1. Launch a SUSE Linux instance \(either 64\-bit or 32\-bit\) and log into it\. 

1. Install PHP dependencies:

   ```
   $ sudo zypper install gcc php53-devel
   ```

1. Download the correct `php-memcached` package for your Amazon EC2 instance and PHP version\. For more information, see [Downloading the installation package](Appendix.PHPAutoDiscoverySetup.Downloading.md)\. 

1. Install `php-memcached`\. The URI should be the download path for the installation package\. 

   ```
   $ sudo pecl install <package download path>
   ```

1. With root/sudo permission, add a new file named `memcached.ini` in the `/etc/php5/conf.d` directory, and insert **extension=`amazon-elasticache-cluster-client.so`** in the file\.

   ```
   $ echo "extension=amazon-elasticache-cluster-client.so" | sudo tee --append /etc/php5/conf.d/memcached.ini
   ```

1. Start or restart your Apache server\.

   ```
   sudo /etc/init.d/httpd start
   ```

**Note**  
If Step 5 doesn't work for any of the previous platforms, verify the install path for `amazon-elasticache-cluster-client.so`\. Also, specify the full path of the binary in the extension\. In addition, verify that the PHP in use is a supported version\. We support versions 5\.3 through 5\.5\. 

 

### Other Linux distributions<a name="Appendix.PHPAutoDiscoverySetup.Installing.PHP5x.Other"></a>

On some systems, notably CentOS7 and Red Hat Enterprise Linux \(RHEL\) 7\.1, `libsasl2.so.3` has replaced `libsasl2.so.2`\. On those systems, when you load the ElastiCache cluster client, it attempts and fails to find and load `libsasl2.so.2`\. To resolve this issue, create a symbolic link to `libsasl2.so.3` so that when the client attempts to load libsasl2\.so\.2, it is redirected to `libsasl2.so.3`\. The following code creates this symbolic link\.

```
cd /usr/lib64
$ sudo ln libsasl2.so.3 libsasl2.so.2
```