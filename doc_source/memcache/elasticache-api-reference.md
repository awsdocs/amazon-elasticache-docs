# Reference<a name="elasticache-api-reference"></a>

The topics in this section cover working with the Amazon ElastiCache API and the ElastiCache section of the AWS CLI\. Also included in this section are common error messages and service notifications\.
+ [Using the ElastiCache API](ProgrammingGuide.md)
+ [ElastiCache API Reference](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/Welcome.html)
+ [ElastiCache section of the AWS CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/elasticache/index.html)
+ [Amazon ElastiCache error messages](ErrorMessages.md)
+ [Notifications](elasticache-notifications.md)

## Setting up the ElastiCache command line interface<a name="StartCLI"></a>

This section describes the prerequisites for running the command line tools, where to get the command line tools, how to set up the tools and their environment, and includes a series of common examples of tool usage\.

Follow the instructions in this topic only if you are going to the AWS CLI for ElastiCache\.

**Important**  
The Amazon ElastiCache Command Line Interface \(CLI\) does not support any ElastiCache improvements after API version 2014\-09\-30\. To use newer ElastiCache functionality from the command line, use the [AWS Command Line Interface](https://aws.amazon.com/cli)\.

**Topics**
+ [Prerequisites](#prerequisites)
+ [Getting the command line tools](#Overview.SetupTools.Getting)
+ [Setting up the tools](#Overview.SetupTools.WhereTheyAre)
+ [Providing credentials for the tools](#Overview.SetupTools.WhoYouAre)
+ [Environmental variables](#Overview.SetupTools.EnvironmentalVariables)

### Prerequisites<a name="prerequisites"></a>

 This document assumes that you can work in a Linux/UNIX or Windows environment\. The Amazon ElastiCache command line tools also work on Mac OS X, which is a UNIX\-based environment; however, no specific Mac OS X instructions are included in this guide\. 

 As a convention, all command line text is prefixed with a generic **PROMPT> **command line prompt\. The actual command line prompt on your machine is likely to be different\. We also use **$ ** to indicate a Linux/UNIX specific command and **C:\\> ** for a Windows specific command\. The example output resulting from the command is shown immediately thereafter without any prefix\. 

#### The Java runtime environment<a name="java-runtime"></a>

 The command line tools used in this guide require Java version 5 or later to run\. Either a JRE or JDK installation is acceptable\. To view and download JREs for a range of platforms, including Linux/UNIX and Windows, see [Java SE Downloads](http://www.oracle.com/technetwork/java/javase/downloads/index.html)\. 

##### Setting the Java home variable<a name="java-home"></a>

 The command line tools depend on an environment variable \(`JAVA_HOME`\) to locate the Java Runtime\. This environment variable should be set to the full path of the directory that contains a subdirectory named `bin` which in turn contains the executable `java` \(on Linux and UNIX\) or `java.exe` \(on Windows\) executable\. 

 **To set the Java Home variable** 

1. Set the Java Home variable\.
   + On Linux and UNIX, enter the following command:

     ```
     $ export JAVA_HOME=<PATH>
     ```
   + On Windows, enter the following command:

     ```
     C:\> set JAVA_HOME=<PATH>
     ```

1.  Confirm the path setting by running **$JAVA\_HOME/bin/java \-version** and checking the output\. 
   + On Linux/UNIX, you will see output similar to the following:

     ```
     $ $JAVA_HOME/bin/java -version
     java version "1.6.0_23"
     Java(TM) SE Runtime Environment (build 1.6.0_23-b05)
     Java HotSpot(TM) Client VM (build 19.0-b09, mixed mode, sharing)
     ```
   + On Windows, you will see output similar to the following:

     ```
     C:\> %JAVA_HOME%\bin\java -version
     java version "1.6.0_23"
     Java(TM) SE Runtime Environment (build 1.6.0_23-b05)
     Java HotSpot(TM) Client VM (build 19.0-b09, mixed mode, sharing)
     ```

### Getting the command line tools<a name="Overview.SetupTools.Getting"></a>

The command line tools are available as a ZIP file on the [ElastiCache Developer Tools web site](https://aws.amazon.com/developertools/Amazon-ElastiCache)\. These tools are written in Java, and include shell scripts for Windows 2000/XP/Vista/Windows 7, Linux/UNIX, and Mac OSX\. The ZIP file is self\-contained and no installation is required; simply download the zip file and unzip it to a directory on your local machine\.

### Setting up the tools<a name="Overview.SetupTools.WhereTheyAre"></a>

The command line tools depend on an environment variable \(AWS\_ELASTICACHE\_HOME\) to locate supporting libraries\. You need to set this environment variable before you can use the tools\. Set it to the path of the directory you unzipped the command line tools into\. This directory is named ElastiCacheCli\-A\.B\.nnnn \(A, B and n are version/release numbers\), and contains subdirectories named bin and lib\.

 **To set the AWS\_ELASTICACHE\_HOME environment variable** 
+ Open a command line window and enter one of the following commands to set the AWS\_ELASTICACHE\_HOME environment variable\.
  + On Linux and UNIX, enter the following command:

    ```
    $ export &AWS;_ELASTICACHE_HOME=<path-to-tools>
    ```
  + On Windows, enter the following command:

    ```
    C:\> set &AWS;_ELASTICACHE_HOME=<path-to-tools>
    ```

To make the tools easier to use, we recommend that you add the tools' BIN directory to your system PATH\. The rest of this guide assumes that the BIN directory is in your system path\.

 **To add the tools' BIN directory to your system path** 
+ Enter the following commands to add the tools' BIN directory to your system PATH\.
  + On Linux and UNIX, enter the following command:

    ```
    $ export PATH=$PATH:$&AWS;_ELASTICACHE_HOME/bin
    ```
  + On Windows, enter the following command:

    ```
    C:\> set PATH=%PATH%;%&AWS;_ELASTICACHE_HOME%\bin
    ```

**Note**  
The Windows environment variables are reset when you close the command window\. You might want to set them permanently\. Consult the documentation for your version of Windows for more information\.

**Note**  
Paths that contain a space must be wrapped in double quotes, for example:  
"C:\\Program Files\\Java"

### Providing credentials for the tools<a name="Overview.SetupTools.WhoYouAre"></a>

 The command line tools need the AWS Access Key and Secret Access Key provided with your AWS account\. You can get them using the command line or from a credential file located on your local system\. 

The deployment includes a template file $\{AWS\_ELASTICACHE\_HOME\}/credential\-file\-path\.template that you need to edit with your information\. Following are the contents of the template file:

```
AWSAccessKeyId=<Write your AWS access ID>
AWSSecretKey=<Write your AWS secret key>
```

**Important**  
On UNIX, limit permissions to the owner of the credential file:  

```
$ chmod 600 <the file created above>
```

With the credentials file setup, you'll need to set the AWS\_CREDENTIAL\_FILE environment variable so that the ElastiCache tools can find your information\.

 **To set the AWS\_CREDENTIAL\_FILE environment variable** 

1. Set the environment variable:
   + On Linux and UNIX, update the variable using the following command:

     ```
     $ export &AWS;_CREDENTIAL_FILE=<the file created above>
     ```
   + On Windows, set the variable using the following command:

     ```
     C:\> set &AWS;_CREDENTIAL_FILE=<the file created above>
     ```

1. Check that your setup works properly, run the following command:

   ```
   elasticache --help
   ```

   You should see the usage page for all ElastiCache commands\.

### Environmental variables<a name="Overview.SetupTools.EnvironmentalVariables"></a>

Environment variables can be useful for scripting, configuring defaults or temporarily overriding them\. 

In addition to the AWS\_CREDENTIAL\_FILE environment variable, most API tools included with the ElastiCache Command Line Interface support the following variables: 
+ **EC2\_REGION** — The AWS region to use\.
+ **AWS\_ELASTICACHE\_URL** — The URL to use for the service call\. Not required to specify a different regional endpoint if EC2\_REGION is specified or the \-\-region parameter is passed\. 

The following examples show how to set the environmental variable EC2\_REGION to configure the region used by the API tools:

Linux, OS X, or Unix

```
1. $ export EC2_REGION=us-west-1 
```

Windows

```
1. $ set EC2_REGION=us-west-1 
```