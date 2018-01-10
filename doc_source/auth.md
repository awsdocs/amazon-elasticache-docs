# Authenticating Users with AUTH \(Redis\)<a name="auth"></a>

Using Redis `AUTH` command can improve data security by requiring the user to enter a password before they are granted permission to execute Redis commands on a password\-protected Redis server\.

This section covers ElastiCache's implementation of Redis' AUTH command, which has some differences from the Redis implementation\.



## Overview of ElastiCache for Redis AUTH<a name="auth-overview"></a>

When using Redis AUTH with your ElastiCache cluster, there are some refinements you need to be aware of\.

**AUTH Token Constraints when using with ElastiCache**

+ Passwords must be at least 16 and a maximum of 128 printable characters\.

+ The printable characters `@`, `"`, and `/` cannot be used in passwords\.

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

## Applying Authentication to an ElastiCache Command<a name="auth-using"></a>

To require that users enter a password on a password\-protected Redis server, add the line `requirepass` in the server's configuration file\. ElastiCache for Redis commands must then include the parameter `--auth-token` \(API: AuthToken\) and the correct password to execute\.

The following AWS CLI operation creates a replication group with Encryption In\-Transit enabled and the AUTH token "this\-is\-a\-sample\-token"\. Replace the subnet group `sng-test` with a subnet group that exists\.

**Key Parameters**

+ `--engine` – must be redis\.

+ `--engine-version` – must be 3\.2\.6\.

+ `--transit-encryption-enabled` – encryption in\-transit must be enabled to use authentication and for HIPAA compliance\.

+ `--auth-token` – must be the correct password for this password\-protected Redis server and must be specified for HIPAA compliance\.

+ `--cache-subnet-group` – must be specified for HIPAA compliance\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
    --replication-group-id authtestgroup \
    --replication-group-description authtest \
    --engine redis \
    --engine-version 3.2.6 \
    --transit-encryption-enabled \
    --cache-node-type cache.m4.large \
    --num-node-groups 1 \
    --replicas-per-node-group 2 \
    --cache-parameter-group default.redis3.2.cluster.on \
    --auth-token This-is-a-sample-token \
    --cache-subnet-group sng-test
```

For Windows:

```
aws elasticache create-replication-group ^
    --replication-group-id authtestgroup ^
    --replication-group-description authtest ^
    --engine redis ^
    --engine-version 3.2.6 ^
    --transit-encryption-enabled ^
    --cache-node-type cache.m4.large ^
    --num-node-groups 1 ^
    --replicas-per-node-group 2 ^
    --cache-parameter-group default.redis3.2.cluster.on ^
    --auth-token This-is-a-sample-token ^
    --cache-subnet-group sng-test
```

## Related topics<a name="auth-see-also"></a>

+ [AUTH password](https://redis.io/commands/auth) at `redis.io`\.