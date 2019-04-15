# Troubleshooting Applications<a name="Troubleshooting"></a>

ElastiCache provides specific and descriptive errors to help you troubleshoot problems while interacting with the ElastiCache API\. 

## Retrieving Errors<a name="RetrievingErrors"></a>

Typically, you want your application to check whether a request generated an error before you spend any time processing results\. The easiest way to find out if an error occurred is to look for an `Error` node in the response from the ElastiCache API\.

XPath syntax provides a simple way to search for the presence of an `Error` node, as well as an easy way to retrieve the error code and message\. The following code snippet uses Perl and the XML::XPath module to determine if an error occurred during a request\. If an error occurred, the code prints the first error code and message in the response\. 

```
use XML::XPath; 
my $xp = XML::XPath->new(xml =>$response); 
if ( $xp->find("//Error") ) 
{print "There was an error processing your request:\n", " Error code: ",
$xp->findvalue("//Error[1]/Code"), "\n", " ",
$xp->findvalue("//Error[1]/Message"), "\n\n"; }
```

## Troubleshooting Tips<a name="Troubleshooting.Tips"></a>

We recommend the following processes to diagnose and resolve problems with the ElastiCache API\. 
+ Verify that ElastiCache is running correctly\.

  To do this, simply open a browser window and submit a query request to the ElastiCache service \(such as https://elasticache\.amazonaws\.com\)\. A MissingAuthenticationTokenException or 500 Internal Server Error confirms that the service is available and responding to requests\.
+ Check the structure of your request\.

  Each ElastiCache operation has a reference page in the *ElastiCache API Reference*\. Double\-check that you are using parameters correctly\. To give you ideas regarding what might be wrong, look at the sample requests or user scenarios to see if those examples are doing similar operations\.
+ Check the forum\.

  ElastiCache has a discussion forum where you can search for solutions to problems others have experienced along the way\. To view the forum, see 

   [https://forums\.aws\.amazon\.com/](https://forums.aws.amazon.com/) \.