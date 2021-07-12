# Authenticating users with the Redis AUTH command<a name="auth"></a>

Redis authentication tokens, or passwords, enable Redis to require a password before allowing clients to run commands, thereby improving data security\.

**Topics**
+ [Overview of AUTH in ElastiCache for Redis](#auth-overview)
+ [Applying authentication to an ElastiCache for Redis cluster](#auth-using)
+ [Modifying the AUTH token on an existing ElastiCache for Redis cluster](#auth-modifyng-token)

## Overview of AUTH in ElastiCache for Redis<a name="auth-overview"></a>

When you use Redis AUTH with your ElastiCache for Redis cluster, there are some refinements\. 

In particular, be aware of these AUTH token, or password, constraints when using AUTH with ElastiCache for Redis:
+ Tokens, or passwords, must be 16–128 printable characters\.
+ Nonalphanumeric characters are restricted to \(\!, &, \#, $, ^, <, >, \-\)\. 
+ AUTH can only be enabled for encryption in\-transit enabled ElastiCache for Redis clusters\.

To set up a strong token, we recommend that you follow a strict password policy, such as requiring the following:
+ Tokens, or passwords, must include at least three of the following character types:
  + Uppercase characters
  + Lowercase characters
  + Digits 
  + Nonalphanumeric characters \(`!`, `&`, `#`, `$`, `^`, `<`, `>`, `-`\)
+ Tokens, or passwords, must not contain a dictionary word or a slightly modified dictionary word\.
+ Tokens, or passwords, must not be the same as or similar to a recently used token\.

## Applying authentication to an ElastiCache for Redis cluster<a name="auth-using"></a>

You can require that users enter a token \(password\) on a token\-protected Redis server\. To do this, include the parameter `--auth-token` \(API: `AuthToken`\) with the correct token when you create your replication group or cluster\. Also include it in all subsequent commands to the replication group or cluster\.

The following AWS CLI operation creates a replication group with encryption in transit \(TLS\) enabled and the AUTH token `This-is-a-sample-token`\. Replace the subnet group `sng-test` with a subnet group that exists\.

**Key parameters**
+ **\-\-engine** – Must be `redis`\.
+ **\-\-engine\-version** – Must be 3\.2\.6, 4\.0\.10, or later\.
+ **\-\-transit\-encryption\-enabled** – Required for authentication and HIPAA eligibility\.
+ **\-\-auth\-token** – Required for HIPAA eligibility\. This value must be the correct token for this token\-protected Redis server\.
+ **\-\-cache\-subnet\-group** – Required for HIPAA eligibility\.

For Linux, OS X, or Unix:

```
aws elasticache create-replication-group \
    --replication-group-id authtestgroup \
    --replication-group-description authtest \
    --engine redis \
    --engine-version 4.0.10 \
    --cache-node-type cache.m4.large \
    --num-node-groups 1 \
    --replicas-per-node-group 2 \
    --cache-parameter-group default.redis3.2.cluster.on \
    --transit-encryption-enabled \
    --auth-token This-is-a-sample-token \
    --cache-subnet-group sng-test
```

For Windows:

```
aws elasticache create-replication-group ^
    --replication-group-id authtestgroup ^
    --replication-group-description authtest ^
    --engine redis ^
    --engine-version 4.0.10 ^
    --cache-node-type cache.m4.large ^
    --num-node-groups 1 ^
    --replicas-per-node-group 2 ^
    --cache-parameter-group default.redis3.2.cluster.on ^ 
    --transit-encryption-enabled ^
    --auth-token This-is-a-sample-token ^
    --cache-subnet-group sng-test
```

## Modifying the AUTH token on an existing ElastiCache for Redis cluster<a name="auth-modifyng-token"></a>

To make it easier to update your authentication, you can modify the AUTH token used on an ElastiCache for Redis cluster\. You can make this modification if the engine version is 5\.0\.5 or higher and if ElastiCache for Redis has encryption in transit enabled\. 

Modifying the auth token supports two strategies: ROTATE and SET\. The ROTATE strategy adds an additional AUTH token to the server while retaining the previous token\. The SET strategy updates the server to support just a single AUTH token\. Make these modification calls with the `--apply-immediately` parameter to apply changes immediately\.

### Rotating the AUTH token<a name="auth-modifyng-rotate"></a>

To update a Redis server with a new AUTH token, call the `ModifyReplicationGroup` API with the `--auth-token` parameter as the new auth token and the `--auth-token-update-strategy` with the value ROTATE\. After the modification is complete, the cluster will support the previous AUTH token in addition to the one specified in the `auth-token` parameter\. 

**Note**  
If you do not configure the AUTH token before, then once the modification is complete, the cluster will support no AUTH token in addition to the one specified in the auth\-token parameter\. 

If this modification is performed on a server that already supports two AUTH tokens, the oldest AUTH token will also be removed during this operation, allowing a server to support up to two most recent AUTH tokens at a given time\.

At this point, you can proceed by updating the client to use the latest AUTH token\. After the clients are updated, you can use the SET strategy for AUTH token rotation \(explained in the following section\) to exclusively start using the new token\. 

The following AWS CLI operation modifies a replication group to rotate the AUTH token `This-is-the-rotated-token`\.

For Linux, macOS, or Unix: 

```
aws elasticache modify-replication-group \
--replication-group-id authtestgroup \
--auth-token This-is-the-rotated-token \
--auth-token-update-strategy ROTATE \
--apply-immediately
```

For Windows:

```
aws elasticache modify-replication-group ^
--replication-group-id authtestgroup ^
--auth-token This-is-the-rotated-token ^
--auth-token-update-strategy ROTATE ^
--apply-immediately
```

### Setting the AUTH token<a name="auth-modifying-set"></a>

To update a Redis server with two AUTH tokens to support a single AUTH token, call the `ModifyReplicationGroup` API operation\. Call `ModifyReplicationGroup` with the `--auth-token` parameter as the new AUTH token and the `--auth-token-update-strategy` parameter with the value SET\. The `auth-token` parameter must be the same value as the last AUTH token rotated\. After the modification is complete, the Redis server supports only the AUTH token specified in the `auth-token` parameter\. 

The following AWS CLI operation modifies a replication group to set the AUTH token to `This-is-the-set-token`\.

For Linux, macOS, or Unix: 

```
aws elasticache modify-replication-group \
--replication-group-id authtestgroup \
--auth-token This-is-the-set-token \
--auth-token-update-strategy SET \
--apply-immediately
```

For Windows:

```
aws elasticache modify-replication-group ^
--replication-group-id authtestgroup ^
--auth-token This-is-the-set-token ^
--auth-token-update-strategy SET ^
--apply-immediately
```

### Enabling authentication on an existing ElastiCache for Redis cluster<a name="auth-enabling"></a>

To enable authentication on an existing Redis server, call the `ModifyReplicationGroup` API operation\. Call `ModifyReplicationGroup` with the `--auth-token` parameter as the new token and the `--auth-token-update-strategy` with the value ROTATE\. 

After the modification is complete, the cluster supports the AUTH token specified in the `auth-token` parameter in addition to supporting connecting without authentication\. Enabling authentication is only supported on Redis servers with encryption in transit \(TLS\) enabled\. 

### Migrating from RBAC to Redis AUTH<a name="Migrate-From-RBAC-to-AUTH"></a>

If you are authenticating users with Redis Role\-Based Access Control \(RBAC\) as described in [Authenticating users with role\-based access control \(RBAC\)](https://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.RBAC.html) and want to migrate to Redis AUTH, use the following procedures\. You can migrate using either console or CLI\.  

**To migrate from RBAC to Redis AUTH using the console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region where the cluster that you want to modify is located\.

1. In the navigation pane, choose the engine running on the cluster that you want to modify\.

   A list of the chosen engine's clusters appears\.

1. In the list of clusters, for the cluster that you want to modify, choose its name\. 

1. For **Actions**, choose **Modify**\. 

   The **Modify Cluster** window appears\.

1. For **Access Control Option**, choose **Redis AUTH Default User**\.

1. Under **AUTH token**, accept either **No change**, rotate an existing token, or set a new token\. 

1. Choose **Modify**\.

**To migrate from RBAC to Redis AUTH using the AWS CLI**
+ Use one of the following commands\.

  For Linux, OS X, or Unix:

  ```
  aws elasticache modify-replication-group \
      --replication-group-id test \
      --remove-user-groups \
      --auth-token password \
      --auth-token-update-strategy SET \ 
      --apply-immediately
  ```

  For Windows:

  ```
  aws elasticache modify-replication-group ^
      --replication-group-id test ^
      --remove-user-groups ^
      --auth-token password ^
      --auth-token-update-strategy SET ^ 
      --apply-immediately
  ```

For more information on working with AUTH, see [AUTH token](https://redis.io/commands/auth) on the redis\.io website\.
