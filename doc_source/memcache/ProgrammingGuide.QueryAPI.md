# Using the query API<a name="ProgrammingGuide.QueryAPI"></a>

## Query parameters<a name="query-parameters"></a>

HTTP Query\-based requests are HTTP requests that use the HTTP verb GET or POST and a Query parameter named `Action`\.

Each Query request must include some common parameters to handle authentication and selection of an action\. 

Some operations take lists of parameters\. These lists are specified using the `param.n` notation\. Values of *n* are integers starting from 1\. 

## Query request authentication<a name="query-authentication"></a>

You can only send Query requests over HTTPS and you must include a signature in every Query request\. This section describes how to create the signature\. The method described in the following procedure is known as *signature version 4*\. 

The following are the basic steps used to authenticate requests to AWS\. This assumes you are registered with AWS and have an Access Key ID and Secret Access Key\. 

**Query authentication process**

1. The sender constructs a request to AWS\.

1. The sender calculates the request signature, a Keyed\-Hashing for Hash\-based Message Authentication Code \(HMAC\) with a SHA\-1 hash function, as defined in the next section of this topic\.

1. The sender of the request sends the request data, the signature, and Access Key ID \(the key\-identifier of the Secret Access Key used\) to AWS\.

1. AWS uses the Access Key ID to look up the Secret Access Key\.

1. AWS generates a signature from the request data and the Secret Access Key using the same algorithm used to calculate the signature in the request\.

1. If the signatures match, the request is considered to be authentic\. If the comparison fails, the request is discarded, and AWS returns an error response\.

**Note**  
If a request contains a `Timestamp` parameter, the signature calculated for the request expires 15 minutes after its value\.   
If a request contains an `Expires` parameter, the signature expires at the time specified by the `Expires` parameter\. 

**To calculate the request signature**

1. Create the canonicalized query string that you need later in this procedure:

   1. Sort the UTF\-8 query string components by parameter name with natural byte ordering\. The parameters can come from the GET URI or from the POST body \(when Content\-Type is application/x\-www\-form\-urlencoded\)\.

   1. URL encode the parameter name and values according to the following rules:

      1. Do not URL encode any of the unreserved characters that RFC 3986 defines\. These unreserved characters are A\-Z, a\-z, 0\-9, hyphen \( \- \), underscore \( \_ \), period \( \. \), and tilde \( \~ \)\.

      1. Percent encode all other characters with %XY, where X and Y are hex characters 0\-9 and uppercase A\-F\.

      1. Percent encode extended UTF\-8 characters in the form %XY%ZA\.\.\.\.

      1. Percent encode the space character as %20 \(and not \+, as common encoding schemes do\)\.

   1. Separate the encoded parameter names from their encoded values with the equals sign \( = \) \(ASCII character 61\), even if the parameter value is empty\.

   1. Separate the name\-value pairs with an ampersand \( & \) \(ASCII code 38\)\.

1. Create the string to sign according to the following pseudo\-grammar \(the "\\n" represents an ASCII newline\)\.

   ```
   StringToSign = HTTPVerb + "\n" +
   ValueOfHostHeaderInLowercase + "\n" +
   HTTPRequestURI + "\n" +
   CanonicalizedQueryString <from the preceding step>
   ```

   The HTTPRequestURI component is the HTTP absolute path component of the URI up to, but not including, the query string\. If the HTTPRequestURI is empty, use a forward slash \( / \)\. 

1. Calculate an RFC 2104\-compliant HMAC with the string you just created, your Secret Access Key as the key, and SHA256 or SHA1 as the hash algorithm\.

   For more information, see [https://www\.ietf\.org/rfc/rfc2104\.txt](https://www.ietf.org/rfc/rfc2104.txt)\.

1. Convert the resulting value to base64\.

1. Include the value as the value of the `Signature` parameter in the request\.

For example, the following is a sample request \(linebreaks added for clarity\)\. 

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeCacheClusters
    &CacheClusterIdentifier=myCacheCluster
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &Version=2014-12-01
```

For the preceding query string, you would calculate the HMAC signature over the following string\. 

```
GET\n
    elasticache.amazonaws.com\n
    Action=DescribeCacheClusters
    &CacheClusterIdentifier=myCacheCluster
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &Version=2014-12-01
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Credential=AKIADQKE4SARGYLE%2F20140523%2Fus-west-2%2Felasticache%2Faws4_request
    &X-Amz-Date=20141201T223649Z
    &X-Amz-SignedHeaders=content-type%3Bhost%3Buser-agent%3Bx-amz-content-sha256%3Bx-amz-date
        content-type:
        host:elasticache.us-west-2.amazonaws.com
        user-agent:CacheServicesAPICommand_Client
    x-amz-content-sha256:
    x-amz-date:
```

The result is the following signed request\. 

```
https://elasticache.us-west-2.amazonaws.com/
    ?Action=DescribeCacheClusters
    &CacheClusterIdentifier=myCacheCluster
    &SignatureMethod=HmacSHA256
    &SignatureVersion=4
    &Version=2014-12-01
    &X-Amz-Algorithm=&AWS;4-HMAC-SHA256
    &X-Amz-Credential=AKIADQKE4SARGYLE/20141201/us-west-2/elasticache/aws4_request
    &X-Amz-Date=20141201T223649Z
    &X-Amz-SignedHeaders=content-type;host;user-agent;x-amz-content-sha256;x-amz-date
    &X-Amz-Signature=2877960fced9040b41b4feaca835fd5cfeb9264f768e6a0236c9143f915ffa56
```

For detailed information on the signing process and calculating the request signature, see the topic [Signature Version 4 Signing Process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) and its subtopics\.