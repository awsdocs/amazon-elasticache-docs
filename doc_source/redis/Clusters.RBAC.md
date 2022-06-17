# Authenticating users with Role\-Based Access Control \(RBAC\)<a name="Clusters.RBAC"></a>

Instead of authenticating users with the Redis AUTH command as described in [Authenticating users with the Redis AUTH command](auth.md), in Redis 6\.0 onward you can use a feature called Role\-Based Access Control \(RBAC\)\. 

Unlike Redis AUTH, where all authenticated clients have full replication group access if their token is authenticated, RBAC enables you to control cluster access through user groups\. These user groups are designed as a way to organize access to replication groups\. 

With RBAC, you create users and assign them specific permissions by using an access string, as described following\. You assign the users to user groups aligned with a specific role \(administrators, human resources\) that are then deployed to one or more ElastiCache for Redis replication groups\. By doing this, you can establish security boundaries between clients using the same Redis replication group or groups and prevent clients from accessing each other’s data\. 

RBAC is designed to support the introduction of [Redis ACL](https://redis.io/docs/manual/security/acl/) in Redis 6\. When you use RBAC with your ElastiCache for Redis cluster, there are some limitations: 
+ You can't specify passwords in an access string\. You set passwords with [CreateUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateUser.html) or [ModifyUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyUser.html) calls\.
+ For user rights, you pass `on` and `off` as a part of the access string\. If neither is specified in the access string, the user is assigned `off` and doesn't have access rights to the replication group\.
+ You can't use forbidden and renamed commands\. If you specify a forbidden or a renamed command, an exception will be thrown\. If you want to use access control lists \(ACLs\) for a renamed command, specify the original name of the command, in other words the name of the command before it was renamed\.
+ You can't use the `reset` command as a part of an access string\. You specify passwords with API parameters, and ElastiCache for Redis manages passwords\. Thus, you can't use `reset` because it would remove all passwords for a user\.
+ Redis 6 introduces the [ACL LIST](https://redis.io/commands/acl-list) command\. This command returns a list of users along with the ACL rules applied to each user\. ElastiCache for Redis supports the `ACL LIST` command, but does not include support for password hashes as Redis does\. With ElastiCache for Redis, you can use the [describe\-users](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-users.html) operation to get similar information, including the rules contained within the access string\. However, [describe\-users](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-users.html) doesn't retrieve a user password\. 

  Other read\-only commands supported by ElastiCache for Redis include [ACL WHOAMI](https://redis.io/commands/acl-whoami), [ACL USERS](https://redis.io/commands/acl-users), and [ACL CAT](https://redis.io/commands/acl-cat)\. ElastiCache for Redis doesn't support any other write\-based ACL commands\.
+ The following constraints apply:    
<a name="quotas-table"></a>[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/Clusters.RBAC.html)

Using RBAC with ElastiCache for Redis is described in more detail following\.

**Topics**
+ [Specifying Permissions Using an Access String](#Access-string)
+ [Applying RBAC to a Replication Group for ElastiCache for Redis](#rbac-using)
+ [Migrating from Redis AUTH to RBAC](#Migrate-From-RBAC-to-Auth)
+ [Migrating from RBAC to Redis AUTH](#Migrate-From-RBAC-to-AUTH-1)

## Specifying Permissions Using an Access String<a name="Access-string"></a>

To specify permissions to an ElastiCache for Redis replication group, you create an access string and assign it to a user, using either the AWS CLI or AWS Management Console\. 

Access strings are defined as a list of space\-delimited rules which are applied on the user\. They define which commands a user can execute and which keys a user can operate on\. In order to execute a command, a user must have access to the command being executed and all keys being accessed by the command\. Rules are applied from left to right cumulatively, and a simpler string may be used instead of the one provided if there are redundancies in the string provided\.

For information about the syntax of the ACL rules, see [ACL](https://redis.io/topics/acl)\. 

In the following example, the access string represents an active user with access to all available keys and commands\.

 `on ~* +@all`

The access string syntax is broken down as follows:
+ `on` – The user is an active user\.
+ `~*` – Access is given to all available keys\.
+ `+@all` – Access is given to all available commands\.

The preceding settings are the least restrictive\. You can modify these settings to make them more secure\.

In the following example, the access string represents a user with access restricted to read access on keys that start with “app::” keyspace

`on ~app::* -@all +@read`

You can refine these permissions further by listing commands the user has access to:

`+command1` – The user's access to commands is limited to *`command1`*\.

 `+@category` – The user's access is limited to a category of commands\.

For information on assigning an access string to a user, see [Creating Users and User Groups with the Console and CLI](#Users-management)\.

If you are migrating an existing workload to ElastiCache, you can retrieve the access string by calling `ACL LIST`, excluding the user and any password hashes\.

## Applying RBAC to a Replication Group for ElastiCache for Redis<a name="rbac-using"></a>

To use ElastiCache for Redis RBAC, you take the following steps: 

1. Create one or more users\.

1. Create a user group and add users to the group\.

1. Assign the user group to a replication group that has in\-transit encryption enabled\.

These steps are described in detail following\.

**Topics**
+ [Creating Users and User Groups with the Console and CLI](#Users-management)
+ [Managing User Groups with the Console and CLI](#User-Groups)
+ [Assigning User Groups to Replication Groups](#Users-groups-to-RGs)

### Creating Users and User Groups with the Console and CLI<a name="Users-management"></a>

The user information for RBAC users is a user ID, user name, and optionally a password and an access string\. The access string provides the permission level on keys and commands\. The user ID is unique to the user, and the user name is what is passed to the engine\. 

Make sure that the user permissions you provide make sense with the intended purpose of the user group\. For example, if you create a user group called `Administrators`, any user you add to that group should have its access string set to full access to keys and commands\. For users in an `e-commerce` user group, you might set their access strings to read\-only access\.

ElastiCache automatically configures a default user with user ID and user name `"default"` and adds it to all user groups\. You can't modify or delete this user\. This user is intended for compatibility with the default behavior of previous Redis versions and has an access string that permits it to call all commands and access all keys\. 

To add proper access control to a cluster, replace this default user with a new one that isn't enabled or uses a strong password\. To change the default user, create a new user with the user name set to `default`\. You can then swap it with the original default user\.

The following procedures shows how to swap the original `default` user with another `default` user that has a modified access string\.

**If you are using the older version of the ElastiCache console:**

**To modify the default user on the console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis** from the navigation pane\.

1. Choose **User Group Management**\.

1. For **User Group ID**, choose the ID that you want to modify\. Make sure that you choose the link and not the check box\.

1. Choose **Swap Default**\.

1. In the **Swap Default** window, for **Default User** choose the user that you want as the default user\.

1. Choose **Swap**\. When you do this, any existing connections to a replication group that the original default user has are terminated\.

**If you are using the new version of the ElastiCache console:**

**To modify the default user on the console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. Choose **Redis** from the navigation pane\.

1. Choose **User Group Management**\.

1. For **User Group ID**, choose the ID that you want to modify\. Make sure that you choose the link and not the check box\.

1. Choose **Modify**\.

1. In the **Modify** window, choose **Manage** and for **Default User** choose the user that you want as the default user\.

1. Choose **Modify**\. When you do this, any existing connections to a replication group that the original default user has are terminated\.

**To modify the default user with the AWS CLI**

1. Create a new user with the user name `default` by using the following commands\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache create-user \
    --user-id "new-default-user" \
    --user-name "default" \
    --engine "REDIS" \
    --passwords "a-str0ng-pa))word" \
    --access-string "off +get ~keys*"
   ```

   For Windows:

   ```
   aws elasticache create-user ^
    --user-id "new-default-user" ^
    --user-name "default" ^
    --engine "REDIS" ^
    --passwords "a-str0ng-pa))word" ^
    --access-string "off +get ~keys*"
   ```

1. Create a user group and add the user that you created previously\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache create-user-group \
     --user-group-id "new-group-2" \
     --engine "REDIS" \
     --user-ids "new-default-user"
   ```

   For Windows:

   ```
   aws elasticache create-user-group ^
     --user-group-id "new-group-2" ^
     --engine "REDIS" ^
     --user-ids "new-default-user"
   ```

1. Swap the new `default` user with the original `default` user\.

   For Linux, macOS, or Unix:

   ```
   aws elasticache modify-user-group \
       --user-group-id test-group \
       --user-ids-to-add "new-default-user" \
       --user-ids-to-remove "default"
   ```

   For Windows:

   ```
   aws elasticache modify-user-group ^
       --user-group-id test-group ^
       --user-ids-to-add "new-default-user" ^
       --user-ids-to-remove "default"
   ```

   When this modify operation is called, any existing connections to a replication group that the original default user has are terminated\.

When creating a user, you can set up to two passwords\. When you modify a password, any existing connections to replication groups are maintained\.

In particular, be aware of these user password constraints when using RBAC for ElastiCache for Redis:
+ Passwords must be 16–128 printable characters\.
+ The following nonalphanumeric characters are not allowed: `,` `""` `/` `@`\. 

#### Managing Users with the Console and CLI<a name="Users-console"></a>

Use the following procedure to manage users on the console\.

**To manage users on the console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the Amazon ElastiCache dashboard, choose **User Management**\. The following options are available:
   + **Create User** – When creating a user, you enter a user ID, user name, password, and access string\. The access string sets the permission level for what keys and commands the user is allowed\. 

     When creating a user, you can set up to two passwords\. When you modify a password, any existing connections to replication groups are maintained\.
   + **Modify User** – Enables you to update a user's password or change its access string\.
   + **Delete User** – The user account will be removed from any User Management Groups to which it belongs\.

Use the following procedures to manage users with the AWS CLI\.

**To modify a user by using the CLI**
+  Use the `modify-user` command to update a user's password or passwords or change a user's access permissions\. 

  When a user is modified, the user groups associated with the user are updated, along with any replication groups associated with the user group\. All existing connections are maintained\. The following are examples\.

  For Linux, macOS, or Unix:

  ```
  aws elasticache modify-user \
    --user-id user-id-1 \
    --access-string "~objects:* ~items:* ~public:*" \
    --no-password-required
  ```

  For Windows:

  ```
  aws elasticache modify-user ^
    --user-id user-id-1 ^
    --access-string "~objects:* ~items:* ~public:*" ^
    --no-password-required
  ```

  The preceding examples return the following response\.

  ```
   {
      "UserId": "user-id-1",
      "UserName": "user-name-1",
      "Status": "modifying",
      "Engine": "redis",
      "AccessString": "off ~objects:* ~items:* ~public:* -@all",
      "UserGroupIds": [
          "new-group-1"
      ],
      "Authentication": {
          "Type": "no-password"
      },
      "ARN": "arn:aws:elasticache:us-east-1:493071037918:user:user-id-1"
  }
  ```

**Note**  
We don't recommend using the `nopass` option\. If you do, we recommend setting the user's permissions to read\-only with access to a limited set of keys\.

**To delete a user by using the CLI**
+ Use the `delete-user` command to delete a user\. The user account is deleted and removed from any user groups to which it belongs\. The following is an example\.

  For Linux, macOS, or Unix:

  ```
  aws elasticache delete-user \
    --user-id user-id-2
  ```

  For Windows:

  ```
  aws elasticache delete-user ^
    --user-id user-id-2
  ```

  The preceding examples return the following response\.

  ```
  {
      "UserId": "user-id-2",
      "UserName": "user-name-2",
      "Status": "deleting",
      "Engine": "redis",
      "AccessString": "on ~keys* -@all +get",
      "UserGroupIds": [
          "new-group-1"
      ],
      "Authentication": {
          "Type": "password",
          "PasswordCount": 1
      },
      "ARN": "arn:aws:elasticache:us-east-1:493071037918:user:user-id-2"
  }
  ```

To see a list of users, call the [describe\-users](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-users.html) operation\.  

```
aws elasticache describe-users
{
    "Users": [
        {
            "UserId": "user-id-1",
            "UserName": "user-id-1",
            "Status": "active",
            "Engine": "redis",
            "AccessString": "+get ~keys*",
            "UserGroupIds": [],
            "Authentication": {
                "Type": "password",
                "PasswordCount": 1
            }
        }
    ]
}
```

### Managing User Groups with the Console and CLI<a name="User-Groups"></a>

You can create user groups to organize and control access of users to one or more replication groups, as shown following\.

Use the following procedure to manage user groups using the console\.

**To manage user groups using the console**

1. Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. On the Amazon ElastiCache dashboard, choose **User Management**\. 

   The following operations are available to create new user groups: 
   + **Create** – When you create a user group, you add users and then assign the user groups to replication groups\. For example, you can create an `Admin` user group for users who have administrative roles on a cluster\.
**Important**  
When you a create user group, you are required to include the default user\.
   + **Add Users** – Add users to the user group\.
   + **Remove Users** – Remove users from the user group\. When users are removed from a user group, any existing connections they have to a replication group are terminated\.
   + **Delete** – Use this to delete a user group\. Note that the user group itself, not the users belonging to the group, will be deleted\.

   For existing user groups, you can do the following:
   + **Add Users** – Add existing users to the user group\.
   + **Delete Users** – Removes existing users from the user group\.
**Note**  
Users are removed from the user group, but not deleted from the system\.

Use the following procedures to manage user groups using the CLI\.

**To create a new user group and add a user by using the CLI**
+ Use the `create-user-group` command as shown following\.

  For Linux, macOS, or Unix:

  ```
  aws elasticache create-user-group \
    --user-group-id "new-group-1" \
    --engine "REDIS" \
    --user-ids user-id-1, user-id-2
  ```

  For Windows:

  ```
  aws elasticache create-user-group ^
    --user-group-id "new-group-1" ^
    --engine "REDIS" ^
    --user-ids user-id-1, user-id-2
  ```

  The preceding examples return the following response\.

  ```
  {
      "UserGroupId": "new-group-1",
      "Status": "creating",
      "Engine": "redis",
      "UserIds": [
          "user-id-1",
          "user-id-2"
      ],
      "ReplicationGroups": [],
      "ARN": "arn:aws:elasticache:us-east-1:493071037918:usergroup:new-group-1"
  }
  ```

**To modify a user group by adding new users or removing current members by using the CLI**
+ Use the `modify-user-group` command as shown following\.

  For Linux, macOS, or Unix:

  ```
  aws elasticache modify-user-group --user-group-id new-group-1 \
  --user-ids-to-add user-id-3 \
  --user-ids-to-remove user-2
  ```

  For Windows:

  ```
  aws elasticache modify-user-group --user-group-id new-group-1 ^
  --user-ids-to-add userid-3 ^
  --user-ids-to-removere user-id-2
  ```

  The preceding examples return the following response\.

  ```
  {
      "UserGroupId": "new-group-1",
      "Status": "modifying",
      "Engine": "redis",
      "UserIds": [
          "user-id-1",
          "user-id-2"
      ],
      "PendingChanges": {
          "UserIdsToRemove": [
              "user-id-2"
          ],
          "UserIdsToAdd": [
              "user-id-3"
          ]
      },
      "ReplicationGroups": [],
      "ARN": "arn:aws:elasticache:us-east-1:493071037918:usergroup:new-group-1"
  }
  ```

**Note**  
Any open connections belonging to a user removed from a user group are ended by this command\.

**To delete a user group by using the CLI**
+ Use the `delete-user-group` command as shown following\. The user group itself, not the users belonging to the group, is deleted\.

  For Linux, macOS, or Unix:

  ```
  aws elasticache delete-user-group /
     --user-group-id
  ```

  For Windows:

  ```
  aws elasticache delete-user-group ^
     --user-group-id
  ```

  The preceding examples return the following response\.

  ```
  aws elasticache delete-user-group --user-group-id "new-group-1"
  {
      "UserGroupId": "new-group-1",
      "Status": "deleting",
      "Engine": "redis",
      "UserIds": [
          "user-id-1", 
          "user-id-3"
      ],
      "ReplicationGroups": [],
      "ARN": "arn:aws:elasticache:us-east-1:493071037918:usergroup:new-group-1"
  }
  ```

To see a list of user groups, you can call the [describe\-user\-groups](https://docs.aws.amazon.com/cli/latest/reference/elasticache/describe-user-groups.html) operation\.

```
aws elasticache describe-user-groups \
  --user-group-id test-group
```

```
{
	"UserGroups": [{
		"UserGroupId": "test-group",
		"Status": "creating",
		"Engine": "redis",
		"UserIds": [
			"defaut", "test-user-1"
		],
		"ReplicationGroups": []
	}]
}
```

### Assigning User Groups to Replication Groups<a name="Users-groups-to-RGs"></a>

After you have created a user group and added users, the final step in implementing RBAC is assigning the user group to a replication group\.

#### Assigning User Groups to Replication Groups Using the Console<a name="Users-groups-to-RGs-CON"></a>

To add a user group to a replication using the AWS Management Console, do the following:
+ For cluster mode disabled, see [Creating a Redis \(cluster mode disabled\) cluster \(Console\)](GettingStarted.CreateCluster.md#Clusters.Create.CON.Redis-gs)
+ For cluster mode enabled, see [Creating a Redis \(cluster mode enabled\) cluster \(Console\)](Clusters.Create.md#Clusters.Create.CON.RedisCluster)

#### Assigning User Groups to Replication Groups Using the AWS CLI<a name="Users-groups-to-RGs-CLI"></a>

 The following AWS CLI operation creates a replication group with encryption in transit \(TLS\) enabled and the user\-group\-ids parameter with the value `my-user-group-id`\. Replace the subnet group `sng-test` with a subnet group that exists\.

**Key Parameters**
+ **\-\-engine** – Must be `redis`\.
+ **\-\-engine\-version** – Must be 6\.0 or later\.
+ **\-\-transit\-encryption\-enabled** – Required for authentication and for associating a user group\.
+ **\-\-user\-group\-ids** – This value provides user groups comprised of users with specified access permissions for the cluster\.
+ **\-\-cache\-subnet\-group** – Required for associating a user group\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
    --replication-group-id "new-replication-group" \
    --replication-group-description "new-replication-group" \
    --engine "redis" \
    --engine-version "6.0" \
    --cache-node-type cache.m5.large \
    --transit-encryption-enabled \
    --user-group-ids "new-group-1" \
    --cache-subnet-group "cache-subnet-group"
```

For Windows:

```
aws elasticache create-replication-group ^
    --replication-group-id "new-replication-group" ^
    --replication-group-description "new-replication-group" ^
    --engine "redis" ^
    --engine-version "6.0" ^
    --cache-node-type cache.m5.large ^
    --transit-encryption-enabled ^
    --user-group-ids "new-group-1" ^
    --cache-subnet-group "cache-subnet-group"
```

The preceding code returns the following response\.

```
{
    "ReplicationGroup": {
        "ReplicationGroupId": "new-replication-group",
        "Description": "new-replication-group",
        "Status": "creating",
        "PendingModifiedValues": {},
        "MemberClusters": [
            "new-replication-group-001"
        ],
        "AutomaticFailover": "disabled",
        "SnapshotRetentionLimit": 0,
        "SnapshotWindow": "10:30-11:30",
        "ClusterEnabled": false,
        "UserGroupIds": ["new-group-1"],
        "CacheNodeType": "cache.m5.large",
        "DataTiering": "disabled"
        "TransitEncryptionEnabled": true,
        "AtRestEncryptionEnabled": false
    }
}
```

The following AWS CLI operation modifies a replication group with encryption in transit \(TLS\) enabled and the user\-group\-ids parameter with the value `my-user-group-id`\. 

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
    --replication-group-id replication-group-1 \
    --user-group-ids-to-remove "new-group-1" \
    --user-group-ids-to-add "new-group-2"
```

For Windows:

```
aws elasticache modify-replication-group ^
    --replication-group-id replication-group-1 ^
    --user-group-ids-to-remove "new-group-1" ^
    --user-group-ids-to-add "new-group-2"
```

The preceding code returns the following response \(abbreviated\):

```
{
  "ReplicationGroupId": "replication-group-1",
  "PendingChanges": {
    "UserGroups": {
       "UserGroupIdsToRemove": ["new-group-1"],
       "UserGroupIdsToAdd": ["new-group-2"]
    }
   }
  .
  .
  .
  "UserGroupIds": ["new-group-1"]
}
```

```
aws elasticache modify-replication-group --replication-group-id replication-group-1 \
--user-group-ids-to-remove "new-group-1" \
--user-group-ids-to-add "new-group-2"

{
  "ReplicationGroupId": "replication-group-1",
  "PendingChanges": {
    "UserGroups": {
       "UserGroupIdsToRemove": ["new-group-1"],
       "UserGroupIdsToAdd": ["new-group-2"]
    }
   }
  .
  .
  .
  "UserGroupIds": ["new-group-1"]
}
```

Note the `PendingChanges` in the response\. Any modifications made to a replication group are updated asynchronously\. You can monitor this progress by viewing the events\. For more information, see [Viewing ElastiCache events](ECEvents.Viewing.md)\.

## Migrating from Redis AUTH to RBAC<a name="Migrate-From-RBAC-to-Auth"></a>

If you are using Redis AUTH as described in [Authenticating users with the Redis AUTH command](auth.md) and want to migrate to using RBAC, use the following procedures\.

Use the following procedure to migrate from Redis AUTH to RBAC using the console\.

**To migrate from Redis AUTH to RBAC using the console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region where the cluster that you want to modify is located\.

1. In the navigation pane, choose the engine running on the cluster that you want to modify\.

   A list of the chosen engine's clusters appears\.

1. In the list of clusters, for the cluster that you want to modify, choose its name\. 

1. For **Actions**, choose **Modify**\. 

   The **Modify Cluster** window appears\.

1. For **Access Control Option**, choose **User Group Access Control List**\.

1.  For **User Group Access Control List**, choose a user group\. 

1. Choose **Modify**\.

Use the following procedure to migrate from Redis AUTH to RBAC using the CLI\.

**To migrate from Redis AUTH to RBAC using the CLI**
+  Use the `modify-replication-group` command as shown following\. 

  For Linux, macOS, or Unix:

  ```
    
  aws elasticache modify-replication-group --replication-group-id test \
      --auth-token-update-strategy DELETE \
      --user-group-ids-to-add user-group-1
  ```

  For Windows:

  ```
    
  aws elasticache modify-replication-group --replication-group-id test ^
      --auth-token-update-strategy DELETE ^
      --user-group-ids-to-add user-group-1
  ```

## Migrating from RBAC to Redis AUTH<a name="Migrate-From-RBAC-to-AUTH-1"></a>

If you are using RBAC and want to migrate to Redis AUTH , see [Migrating from RBAC to Redis AUTH](auth.md#Migrate-From-RBAC-to-AUTH)\. 
