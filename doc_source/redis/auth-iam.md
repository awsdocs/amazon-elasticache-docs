# Authenticating with IAM<a name="auth-iam"></a>

**Topics**
+ [Overview](#auth-iam-overview)
+ [Limitations](#auth-iam-limits)
+ [Setup](#auth-iam-setup)
+ [Connecting](#auth-iam-Connecting)

## Overview<a name="auth-iam-overview"></a>

With IAM Authentication you can authenticate a connection to ElastiCache for Redis using AWS IAM identities, when your cluster is configured to use Redis version 7 or above\. This allows you to strengthen your security model and simplify many administrative security tasks\. With IAM Authentication you can configure fine\-grained access control for each individual ElastiCache replication group and ElastiCache user and follow least\-privilege permissions principles\. IAM Authentication for ElastiCache Redis works by providing a short\-lived IAM authentication token instead of a long\-lived ElastiCache user password in the Redis `AUTH` or `HELLO` command\. 

With IAM Authentication you can authenticate a connection to ElastiCache for Redis using AWS IAM identities, when your cluster is configured to use Redis version 7 or above\. This allows you to strengthen your security model and simplify many administrative security tasks\. With IAM Authentication you can configure fine\-grained access control for each individual ElastiCache replication group and ElastiCache user and follow least\-privilege permissions principles\. IAM Authentication for ElastiCache Redis works by providing a short\-lived IAM authentication token instead of a long\-lived ElastiCache user password in the Redis `AUTH` or `HELLO` command\. For more information about the IAM authentication token, refer to the [Signature Version 4 signing process](https://docs.aws.amazon.com/general/latest/gr/signature-version-4.html) in the the AWS General Reference Guide and the code example below\. 

You can use IAM identities and their associated policies to further restrict Redis access\. You can also grant access to users from their federated Identity providers directly to Redis clusters\.

To use AWS IAM with ElastiCache for Redis, you first need to create an ElastiCache user with authentication mode set to IAM, then you can create or reuse an IAM identity\. The IAM identity needs an associated policy to grant the `elasticache:Connect` action to the ElastiCache replication group and ElastiCache user\. Once configured, you can create an IAM authentication token using the AWS credentials of the IAM user or role\. Finally you need to provide the short\-lived IAM authentication token as a password in your Redis Client when connecting to your Redis replication group node\. A Redis client with support for credentials provider can auto\-generate the temporary credentials automatically for each new connection\. ElastiCache for Redis will perform IAM authentication for connection requests of IAM\-enabled ElastiCache users and will validate the connection requests with IAM\. 

## Limitations<a name="auth-iam-limits"></a>

When using IAM authentication, the following limitations apply:
+ IAM authentication is available when using ElastiCache for Redis version 7\.0 or above\.
+ For IAM\-enabled ElastiCache users the username and user id properties must be identical\.
+ The IAM authentication token is valid for 15 minutes\. For long\-lived connections, we recommend using a Redis client that supports a credentials provider interface\.
+ An IAM authenticated connection to ElastiCache for Redis will automatically be disconnected after 12 hours\. The connection can be prolonged for 12 hours by sending an `AUTH` or `HELLO` command with a new IAM authentication token\.
+ IAM authentication is not supported in `MULTI EXEC` commands\.

## Setup<a name="auth-iam-setup"></a>

To setup IAM authentication:

1. Create a cache cluster

   ```
   aws elasticache create-replication-group \
     --replication-group-id replication-group-01  \
     --replication-group-description "ElastiCache IAM auth application" \
     --engine redis \
     --engine-version 7.0 \
     --cache-node-type cache.m5.large \
     --transit-encryption-enabled \
     --cache-subnet-group insert cache security group
   ```

1. Create an IAM trust policy document, as shown below, for your role that allows your account to assume the new role\. Save the policy to a file named *trust\-policy\.json*\.

   ```
   {
       "Version": "2012-10-17",
       "Statement": {
           "Effect": "Allow",
           "Principal": { "AWS": "arn:aws:iam::123456789012:root" },
           "Action": "sts:AssumeRole"
       }
   }
   ```

1. Create an IAM policy document, as shown below\. Save the policy to a file named *policy\.json*\.

   ```
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect" : "Allow",
         "Action" : [
           "elasticache:Connect"
         ],
         "Resource" : [
           "arn:aws:elasticache:us-east-1:123456789012:replicationgroup:replication-group-01",
           "arn:aws:elasticache:us-east-1:123456789012:user:iam-user-01"
         ]
       }
     ]
   }
   ```

1. Create an IAM role\.

   ```
   aws iam create-role \
   --role-name "elasticache-iam-auth-app" \
   --assume-role-policy-document file://trust-policy.json
   ```

1. Create the IAM policy\.

   ```
   aws iam create-policy \
     --policy-name "elasticache-allow-all" \
     --policy-document file://policy.json
   ```

1. Attach the IAM policy to the role\.

   ```
   aws iam attach-role-policy \
    --role-name "elasticache-iam-auth-app" \
    --policy-arn "arn:aws:iam::123456789012:policy/elasticache-allow-all"
   ```

1. Create a new IAM\-enabled user\.

   ```
   aws elasticache create-user \
     --user-name iam-user-01 \
     --user-id iam-user-01 \
     --authentication-mode Type=iam \
     --engine redis \
     --access-string "on ~* +@all"
   ```

1. Create a user group and attach the user\.

   ```
   aws elasticache create-user-group \
     --user-group-id iam-user-group-01 \
     --engine redis \
     --user-ids default iam-user-01
   
   aws elasticache modify-replication-group \
     --replication-group-id replication-group-01  \
     --user-group-ids-to-add iam-user-group-01
   ```

## Connecting<a name="auth-iam-Connecting"></a>

**Connect with token as password**

You first need to generate the short\-lived IAM authentication token using an [AWS SigV4 pre\-signed request](https://docs.aws.amazon.com/general/latest/gr/sigv4-signed-request-examples.html)\. After that you provide the IAM authentication token as a password when connecting to a Redis cluster, as shown in the example below\. 

```
String userId = "insert user id"
String replicationGroupId = "insert replication group id"
String region = "insert region"

// Create a default AWS Credentials provider.
// This will look for AWS credentials defined in environment variables or system properties.
AWSCredentialsProvider awsCredentialsProvider = new DefaultAWSCredentialsProviderChain();

// Create an IAM authentication token request and signed it using the AWS credentials.
// The pre-signed request URL is used as an IAM authentication token for Elasticache Redis.
IAMAuthTokenRequest iamAuthTokenRequest = new IAMAuthTokenRequest(userId, replicationGroupId, region);
String iamAuthToken = iamAuthTokenRequest.toSignedRequestUri(awsCredentialsProvider.getCredentials());

// Construct Redis URL with IAM Auth credentials provider
RedisURI redisURI = RedisURI.builder()
    .withHost(host)
    .withPort(port)
    .withSsl(ssl)
    .withAuthentication(userId, iamAuthToken)
    .build();

// Create a new Lettuce Redis client
RedisClient client = RedisClient.create(redisURI);
client.connect();
```

Below is the definition for `IAMAuthTokenRequest`\.

```
public class IAMAuthTokenRequest {
    private static final HttpMethodName REQUEST_METHOD = HttpMethodName.GET;
    private static final String REQUEST_PROTOCOL = "http://";
    private static final String PARAM_ACTION = "Action";
    private static final String PARAM_USER = "User";
    private static final String ACTION_NAME = "connect";
    private static final String SERVICE_NAME = "elasticache";
    private static final long TOKEN_EXPIRY_SECONDS = 900;

    private final String userId;
    private final String replicationGroupId;
    private final String region;

    public IAMAuthTokenRequest(String userId, String replicationGroupId, String region) {
        this.userId = userId;
        this.replicationGroupId = replicationGroupId;
        this.region = region;
    }

    public String toSignedRequestUri(AWSCredentials credentials) throws URISyntaxException {
        Request<Void> request = getSignableRequest();
        sign(request, credentials);
        return new URIBuilder(request.getEndpoint())
            .addParameters(toNamedValuePair(request.getParameters()))
            .build()
            .toString()
            .replace(REQUEST_PROTOCOL, "");
    }

    private <T> Request<T> getSignableRequest() {
        Request<T> request  = new DefaultRequest<>(SERVICE_NAME);
        request.setHttpMethod(REQUEST_METHOD);
        request.setEndpoint(getRequestUri());
        request.addParameters(PARAM_ACTION, Collections.singletonList(ACTION_NAME));
        request.addParameters(PARAM_USER, Collections.singletonList(userId));
        return request;
    }

    private URI getRequestUri() {
        return URI.create(String.format("%s%s/", REQUEST_PROTOCOL, replicationGroupId));
    }

    private <T> void sign(SignableRequest<T> request, AWSCredentials credentials) {
        AWS4Signer signer = new AWS4Signer();
        signer.setRegionName(region);
        signer.setServiceName(SERVICE_NAME);

        DateTime dateTime = DateTime.now();
        dateTime = dateTime.plus(Duration.standardSeconds(TOKEN_EXPIRY_SECONDS));

        signer.presignRequest(request, credentials, dateTime.toDate());
    }

    private static List<NameValuePair> toNamedValuePair(Map<String, List<String>> in) {
        return in.entrySet().stream()
            .map(e -> new BasicNameValuePair(e.getKey(), e.getValue().get(0)))
            .collect(Collectors.toList());
    }
}
```

**Connect with credentials provider**

The code below shows how to authenticate with ElastiCache for Redis using the IAM authentication credentials provider\.

```
String userId = "insert user id"
String replicationGroupId = "insert replication group id"
String region = "insert region"

// Create a default AWS Credentials provider.
// This will look for AWS credentials defined in environment variables or system properties.
AWSCredentialsProvider awsCredentialsProvider = new DefaultAWSCredentialsProviderChain();

// Create an IAM authentication token request. Once this request is signed it can be used as an
// IAM authentication token for Elasticache Redis.
IAMAuthTokenRequest iamAuthTokenRequest = new IAMAuthTokenRequest(userId, replicationGroupId, region);

// Create a Redis credentials provider using IAM credentials.
RedisCredentialsProvider redisCredentialsProvider = new RedisIAMAuthCredentialsProvider(
    userId, iamAuthTokenRequest, awsCredentialsProvider);
    
// Construct Redis URL with IAM Auth credentials provider
RedisURI redisURI = RedisURI.builder()
    .withHost(host)
    .withPort(port)
    .withSsl(ssl)
    .withAuthentication(redisCredentialsProvider)
    .build();

// Create a new Lettuce Redis client
RedisClient client = RedisClient.create(redisURI);
client.connect();
```

Below is an example of a Lettuce Redis client that wraps the IAMAuthTokenRequest in a credentials provider to auto\-generate temporary credentials when needed\.

```
public class RedisIAMAuthCredentialsProvider implements RedisCredentialsProvider {
    private static final long TOKEN_EXPIRY_SECONDS = 900;

    private final AWSCredentialsProvider awsCredentialsProvider;
    private final String userId;
    private final IAMAuthTokenRequest iamAuthTokenRequest;
    private final Supplier<String> iamAuthTokenSupplier;

    public RedisIAMAuthCredentialsProvider(String userId,
        IAMAuthTokenRequest iamAuthTokenRequest,
        AWSCredentialsProvider awsCredentialsProvider) {
        this.userName = userName;
        this.awsCredentialsProvider = awsCredentialsProvider;
        this.iamAuthTokenRequest = iamAuthTokenRequest;      
        this.iamAuthTokenSupplier = Suppliers.memoizeWithExpiration(this::getIamAuthToken, TOKEN_EXPIRY_SECONDS, TimeUnit.SECONDS);
    }

    @Override
    public Mono<RedisCredentials> resolveCredentials() {
        return Mono.just(RedisCredentials.just(userId, iamAuthTokenSupplier.get()));
    }

    private String getIamAuthToken() {
        return iamAuthTokenRequest.toSignedRequestUri(awsCredentialsProvider.getCredentials());
    }
}
```