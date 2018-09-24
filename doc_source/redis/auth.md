# Authenticating Users with Redis AUTH<a name="auth"></a>

Using Redis `AUTH` command can improve data security by requiring the user to enter a password before they are granted permission to execute Redis commands on a password\-protected Redis server\.

This section covers ElastiCache for Redis's implementation of Redis' AUTH command, which has some differences from the Redis implementation\.

**Contents**
+ [Overview of Amazon ElastiCache for Redis AUTH](#auth-overview)
+ [Applying Authentication to an ElastiCache for Redis Command](#auth-using)
+ [Related topics](#auth-see-also)

## Overview of Amazon ElastiCache for Redis AUTH<a name="auth-overview"></a>

When using Redis AUTH with your ElastiCache for Redis cluster, there are some refinements you need to be aware of\.

**AUTH Token Constraints when using with Amazon ElastiCache for Redis**
+ Passwords must be at least 16 and a maximum of 128 printable characters\.
+ The only permitted printable special characters are `!`, `&`, `#`, `$`, `^`, `<`, `>`, and `-`\. Other printable special characters cannot be used in the AUTH token\.
+ AUTH can only be enabled when creating clusters where in\-transit encryption is enabled\.
+ The password set at cluster creation cannot be changed\.

We recommend that you follow a stricter policy such as:
+ Must include a mix of characters that includes at least three of the following character types:
  + Uppercase characters
  + Lowercase characters
  + Digits 
  + Non\-alphanumeric characters \(`!`, `&`, `#`, `$`, `^`, `<`, `>`, `-`\)
+ Must not contain a dictionary word or a slightly modified dictionary word\.
+ Must not be the same as or similar to a recently used password\.

## Applying Authentication to an ElastiCache for Redis Command<a name="auth-using"></a>

To require that users enter a password on a password\-protected Redis server, include the parameter `--auth-token` \(API: `AuthToken`\) with the correct password when you create your replication group or cluster and on all subsequent commands to the replication group or cluster\.

The following AWS CLI operation creates a replication group with Encryption In\-Transit \(TLS\) enabled and the AUTH token "This\-is\-a\-sample\-token"\. Replace the subnet group `sng-test` with a subnet group that exists\.

**Key Parameters**
+ **\-\-engine**—Must be `redis`\.
+ **\-\-engine\-version**—Must be 3\.2\.6, 4\.0\.10 or later\.
+ **\-\-transit\-encryption\-enabled**—Required for authentication and HIPAA compliance\.
+ **\-\-auth\-token**—Required for HIPAA compliance\. Must be the correct password for this password\-protected Redis server\.
+ **\-\-cache\-subnet\-group**—Required for HIPAA compliance\.

For Linux, macOS, or Unix:

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

## Related topics<a name="auth-see-also"></a>
+ [AUTH password](https://redis.io/commands/auth) at `redis.io`\.