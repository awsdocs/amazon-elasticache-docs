# Installing the ElastiCache Cluster Client for \.NET<a name="Appendix.DotNETAutoDiscoverySetup"></a>

You can find the ElastiCache \.NET Cluster Client code as open source at [https://github\.com/awslabs/elasticache\-cluster\-config\-net](https://github.com/awslabs/elasticache-cluster-config-net)\.

This section describes how to install, update, and remove the \.NET components for the ElastiCache Cluster Client on Amazon EC2 instances\. For more information about auto discovery, see [Node Auto Discovery \(Memcached\)](AutoDiscovery.md)\. For sample \.NET code to use the client, see [Using the ElastiCache Cluster Client for \.NET](AutoDiscovery.Using.md#AutoDiscovery.Using.ModifyApp.DotNET)\.


+ [Installing \.NET](#Appendix.DotNETAutoDiscoverySetup.DotNET)
+ [Download the ElastiCache \.NET Cluster Client for ElastiCache](#Appendix.DotNETAutoDiscoverySetup.Downloading)
+ [Install AWS Assemblies with NuGet](#Appendix.DotNETAutoDiscoverySetup.Installing)

## Installing \.NET<a name="Appendix.DotNETAutoDiscoverySetup.DotNET"></a>

You must have \.NET 3\.5 or later installed to use the AWS \.NET SDK for ElastiCache\. If you don't have \.NET 3\.5 or later, you can download and install the latest version from [http://www\.microsoft\.com/net](http://www.microsoft.com/net)\.

## Download the ElastiCache \.NET Cluster Client for ElastiCache<a name="Appendix.DotNETAutoDiscoverySetup.Downloading"></a>

**To download the ElastiCache \.NET cluster client**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the navigation pane, click **ElastiCache Cluster Client**\.

1. In the **Download ElastiCache Memcached Cluster Client** list, select **\.NET**, and then click **Download\.**

## Install AWS Assemblies with NuGet<a name="Appendix.DotNETAutoDiscoverySetup.Installing"></a>

NuGet is a package management system for the \.NET platform\. NuGet is aware of assembly dependencies and installs all required files automatically\. NuGet installed assemblies are stored with your solution, rather than in a central location such as `Program Files`, so you can install versions specific to an application without creating compatibility issues\.

### Installing NuGet<a name="Appendix.DotNETAutoDiscoverySetup.Installing.NuGet"></a>

NuGet can be installed from the Installation Gallery on MSDN; go to [https://visualstudiogallery\.msdn\.microsoft\.com/27077b70\-9dad\-4c64\-adcf\-c7cf6bc9970c](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)\. If you are using Visual Studio 2010 or later, NuGet is automatically installed\.

You can use NuGet from either **Solution Explorer** or **Package Manager Console**\.

### Using NuGet from Solution Explorer<a name="Appendix.DotNETAutoDiscoverySetup.NuGet.SolutionExplorer"></a>

**To use NuGet from Solution Explorer in Visual Studio 2010**

1. From the **Tools** menu, select **Library Package Manager**\.

1. Click **Package Manager Console**\.

**To use NuGet from Solution Explorer in Visual Studio 2012 or Visual Studio 2013**

1. From the **Tools** menu, select **NuGet Package Manager**\.

1. Click **Package Manager Console**\.

From the command line, you can install the assemblies using `Install-Package`, as shown following\.

```
Install-Package Amazon.ElastiCacheCluster
```

To see a page for every package that is available through NuGet, such as the AWSSDK and AWS\.Extensions assemblies, go to the NuGet website at [http://www\.nuget\.org](http://www.nuget.org)\. The page for each package includes a sample command line for installing the package using the console and a list of the previous versions of the package that are available through NuGet\.

For more information on **Package Manager Console** commands, go to [http://nuget\.codeplex\.com/wikipage?title=Package%20Manager%20Console%20Command%20Reference%20%28v1\.3%29](http://nuget.codeplex.com/wikipage?title=Package%20Manager%20Console%20Command%20Reference%20%28v1.3%29)\.