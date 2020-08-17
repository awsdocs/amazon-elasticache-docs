# Authenticating Users with Role Based Access Control<a name="Clusters.RBAC"></a>

An alternative to [Authenticating Users with the Redis AUTH Command](auth.md), Amazon ElastiCache for Redis 6\.x offers Role Based Access Control \(RBAC\)\. Unlike Redis AUTH, wherein all authenticated clients have full replication group access if their token is authenticated, RBAC allows you to control cluster access through user groups\. With RBAC, you create users set specific permissions for access to a Redis replication group\. You assign the users to user groups, such as administrators, readers etc\., which in turn are deployed to one or more replication groups\. User groups are designed as a way to organize access to replication groups\. This allows you to establish security boundaries between clients using the same Redis replication group\(s\) and prevent clients from accessing each other’s data\. 

RBAC is designed to support the introduction of [Redis ACL](https://redis.io/topics/acl) in Redis 6\. When you use RBAC with your ElastiCache for Redis cluster, there are some refinements\. 
+ You won't be able to specify passwords inside of an access string\. You must use passwords set with [CreateUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_CreateUser.html) or [ModifyUser](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_ModifyUser.html) calls\.
+ You are allowed to pass `on` and `off` as a part of the access string\. If neither is specified in the access string, the user will be assigned `off` and will not have access rights to the replication group\.
+ Forbidden and renamed commands are not allowed\. If you specify a forbidden or a renamed command, an exception will be thrown\. If you want to use ACLs for a renamed command, specify the original name of the commend, i\.e\. the name of the command before it was renamed\.
+ The `reset` command won't be allowed as a part of an access string\. Since you are specifying passwords with API parameters, and ElastiCache for Redis is managing passwords, ElastiCache for Redis can't allow this command since it would remove all passwords for a user\.
+ Redundant commands are allowed for the access string\. For example if an access string contains `+get -get +get*`, this will be reduced to `+get`\. 
+ Redis 6 introduces the [ACL LIST](https://redis.io/commands/acl-list) command\. This command returns a list of users along with the ACL rules applied to that user\. ElastiCache for Redis offers the [DescribeUsers](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUsers.html) API, which returns similar information, including the rules contained within the acccess string\. The exception is [DescribeUsers](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUsers.html) will not expose a user password\. 

## Applying RBAC to an ElastiCache for Redis Replication Group<a name="rbac-using"></a>

Using RBAC for ElastiCache for Redis involves the following steps: 
+ Create one or more users\.
+ Create a user group and add users to the group\.
+ Assign the user group to a replication group that has in\-transit encryption enabled\.

### User Management<a name="Users-management"></a>

Users are comprised of a User ID, User Name, optionally a password and an Access String, which is the permission level on keys and commands\. User permissions should be aligned with the intended purpose of the user group\. For example, if you create a user group called **Administrators**, any user you add to that group should have its access string set to full access to keys and commands\. Conversely, users added to a **Readers** user group should have their access strings limited to read\-only access\.

**Default User**

In order to maintain compatibility with Redis 6, ElastiCache for Redis automatically configures a default user account with the user ID and user name `default`\. The default user isn’t modifiable and can't be deleted\. Its access string permits it to call all commands and access all keys, which represents a significant security risk that we don't recommend\. If you wants to change the behaviour of the default user, you need to create a new user with the user name set to `default`\. You then swap it with the original `default` account\. User names need not be unique when creating users, but must be unique within a user group\. In this case, you can restrict the permissions the `default` user has on the replication group\. The following example shows how to swap the original `default` user with another `default` that has a modified access string\. \(Access strings are discussed further in the next section\)\.

****

1. Create a new user with the user name `default`\.

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

1. Create a user group and add the user you created previously\.

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
       --user-ids-to-removere "default-user"
   ```

   For Linux, macOS, or Unix:

   ```
   aws elasticache modify-user-group ^
       --user-group-id test-group ^
       --user-ids-to-add "new-default-user" ^
       --user-ids-to-removere "default-user"
   ```

   When this modify operation is called, any existing connections to a replication group that the original default user has will be terminated\.

 **Access string**

As noted, the access string is where you specify access settings\. 
+ `on`  User is active\.
+ `nopass`  Password is not required\.
+ `~*`  Access to all available keys\.
+ `+@all`  Access to all available commands\.

The above settings are the least restrictive\. You can modify these settings to make them more secure, as shown following:
+ `off`  User is inactive\. This means the user cannot be authenticated\. Any existing connections will be maintained\.
+ `pass`  Password is required\.
+ `+get ~keys*`  Read\-only access to all available keys\.
+ `-@all`  Access to no commands\.

You can refine these permissions further by listing commands the user has access to:

`+@command1`  The user's access to commands is limited to *command1*\.

For more information, see [ACL](https://redis.io/topics/acl)\.

When creating a user, you can set up to two passwords\. Note that when you modify a password, any existing connections to replication groups will be maintained\.

In particular, be aware of these user password constraints when using RBAC for ElastiCache for Redis:
+ Passwords must be 16–128 printable characters\.
+ Nonalphanumeric characters are restricted to \(\!, &, \#, $, ^, <, >, \-\)\. 

#### User Management Using the AWS Management Console<a name="Users-console"></a>

Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

On the Amazon ElastiCache Dashboard, select **User Management**\. The following operations are available:
+ **Create User** – When creating a user, you enter a User ID, User Name, Password and Access String, which is the permission level on keys and commands the user is allowed\. 

  When creating a user, you can set up to two passwords\. Note that when you modify a password, any existing connections to replication groups will be maintained\.

  In particular, be aware of these user password constraints when using RBAC for ElastiCache for Redis:
  + Passwords must be 16–128 printable characters\.
  + Nonalphanumeric characters are restricted to \(\!, &, \#, $, ^, <, >, \-\)\. 
+ **Modify User** – Allows you to update a user's password or change its access permissions\.
+ **Delete User** – The user account will be removed from any User Management Groups to which it belongs\.

#### User Management Using the AWS CLI<a name="Users-cli"></a>
+ **Create User** – When creating a user, you enter a User ID, User Name, Password and Access String, which is the permission level on operations the user is allowed\. 

  The following example creates a user with a user\-id *user\-id\-1* and a user\-name *default*\. In this example, you would swap this user account with the automatically configured `default` account provided by ElastiCache for Redis\.

  For Linux, macOS, or Unix:

  ```
  aws elasticache create-user \
   --user-id user-id-1 \
   --user-name user-id-1 \
   --engine "REDIS" \
   --passwords "strongly-fOrmed-passw%rd" \
   --access-string "+get ~keys*"
  ```

  For Windows:

  ```
  aws elasticache create-user ^
   --user-id user-id-1 ^
   --user-name user-id-1 ^
   --engine "REDIS" ^
   --passwords "strongly-fOrmed-passw%rd" ^
   --access-string "+get ~keys*"
  ```

  This will return the following response:

  ```
  {
    "UserId": "user-id-1",
    "UserName": "user-id-1",
    "Status": "CREATING",
    "Engine": "REDIS",
    "AccessString": "+get ~keys*",
    "UserGroupIds": "[],"
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:user:user-id-1",
    "Authentication": {
      "Type": "password",
      "PasswordCount": 1
    } 
  }
  ```
+ **Modify User** – Allows you to update a user's password\(s\) or change its access permissions\. When a user is modified, the user groups associated with the user will be updated, along with any replication groups associated with the user group\. All existing connections will be maintained\.

  The following is an example:

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

  This will return the following response:

  ```
  {
    "UserId": "user-id-1",
    "UserName": "default",
    "Status": "MODIFYING",
    "Engine": "REDIS",
    "AccessString": "~objects:* ~items:* ~public:*",
    "UserGroupIds": "["user-group-1", "user-group-2"]",
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:user:user-123",
    "Authentication": {
      "Type": "no-password"
    }
  }
  ```
**Note**  
We don't recommend using the `nopass` option\. If you do, we recommend setting the user's permissions to read\-only with access to a limited set of keys\.
+ **Delete User** – The user account will be deleted and removed from any User Management Groups to which it belongs\.

  The following is an example:

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

  This will return the following response:

  ```
  {
    "UserId": "user-id-2",
    "UserName": user-name-2,
    "Status": "deleting",
    "Engine": "redis",
    "AccessString": "~objects:* ~items:* ~public:* +set +get",
    "UserGroupIds": "["user-group-1", "user-group-2"]",
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:user:user-123",
    "Authentication": {
       "Type": "password",
       "PasswordCount": 1
    } 
  }
  ```

To see a list of users, you can call the [DescribeUsers](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUsers.html) API:

```
aws elasticache describe-users
{
    "Users": [
        {
            "UserId": "user-id-1",
            "UserName": "user-id-1",
            "Status": "active",
            "Engine": "REDIS",
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

### User Group Management<a name="Users-groups"></a>

You create user groups to organize and control access of users to one or more replication groups, as shown following: 

#### User Group Management Using the AWS Management Console<a name="User-Groups-console"></a>

Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

On the Amazon ElastiCache Dashboard, select **User Management**\. The following operations are available:
+ **Create** – When you create a user group, you add users and then assign the user groups to replication groups\. For example, you could create an **Admin** user group for users who have administrative roles on a cluster\.
**Important**  
When you a create user group, you are required to include the default user\.
+ **Swap** – **Add** and/or **Remove** users from the user group\. When users are removed from a user group, any existing connections they have to a replication group are terminated\.
+ **Delete** – Use this to delete a user group\. Note that the user group itself, not the users belonging to the group, will be deleted\.

For existing user goups, you can do the following:
+ **Add Users** – Add existing users to the user group\.
+ **Delete Users** – Removes existing users from the user group\.
**Note**  
Users are removed from the user group, but not deleted from the system\.

#### User Group Management Using the AWS CLI<a name="User-Groups-cli"></a>
+ **Create** 

  The following example creates a new user group and adds one user:

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

  This will return the following response:

  ```
  {
    "UserGroupId": "new-group-1",
    "Status": "CREATING",
    "Engine": "REDIS",
    "UserIds": ["user-id-1, user-id-2"],
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:usergroup:new-group-1" 
  }
  ```
+ **Modify** – You can modify a user group by adding new users and/or removing current members\. 

  The following is an example:

  For Linux, macOS, or Unix:

  ```
  aws elasticache modify-user-group --user-group-id new-group-1 \
  --user-ids-to-add user-id-3 \
  --user-ids-to-removere user-2
  ```

  For Windows:

  ```
  aws elasticache modify-user-group --user-group-id new-group-1 ^
  --user-ids-to-add userid-3 ^
  --user-ids-to-removere user-id-2
  ```

  This will return the following response:

  ```
  {
    "UserGroupId": "new-group-1",
    "Status": "MODIFYING",
    "Engine": "REDIS",
    "UserIds": ["user-id-1", "user-id-2"],
    "PendingChanges:" {
       "UserIdsToRemove": ["user-id-2"],
       "UserIdsToAdd": ["user-id-3"]
    },
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:usergroup:new-group-1"
  }
  ```
**Note**  
Any open connections belonging to a user removed from a user group will be terminated by this call\.
+ **Delete** – Use this to delete a user group\. Note that the user group itself, not the users belonging to the group, will be deleted\.

  The following is an example:

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

  This will return the following response:

  ```
  aws elasticache delete-user-group --user-group-id "new-group-1"
  
  {
    "UserGroupId": "new-group-1",
    "Status": "DELETING",
    "Engine": "REDIS",
    "UserIds": ["user-id-1", "user-id-3"],
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:usergroup:retail" 
  }
  ```
+ To see a list of user groups, you can call the [DescribeUserGroups](https://docs.aws.amazon.com/AmazonElastiCache/latest/APIReference/API_DescribeUserGroups.html) API:

  ```
  aws elasticache describe-user-groups \
  --user-group-id test-group
  	 {
  	     "UserGroups": [
  			    {
  			       "UserGroupId": "test-group",
  			       "Status": "creating",
  			       "Engine": "REDIS",
  			       "UserIds": [
  			       "defaut", "test-user-1"
  			    ],
  			    "ReplicationGroups": []
  			   }
  			 ]
  		}
  ```

### Assigning User Groups to Replication Groups<a name="Users-groups-to-RGs"></a>

Once you have created a user group and added users, the final step in implementing RBAC is assigning the user group to a replication group\.

#### Assigning User Groups to Replication Groups Using the AWS Management Console<a name="Users-groups-to-RGs-CON"></a>

To add a user group to a replication using the AWS Management Console, do the following:
+ For cluster mode disabled, see [Creating a Redis \(cluster mode disabled\) Cluster \(Console\)](Clusters.Create.CON.Redis.md)
+ For cluster mode ensabled, see [Creating a Redis \(Cluster Mode Enabled\) Cluster \(Console\)](Clusters.Create.CON.RedisCluster.md)

#### Assigning User Groups to Replication Groups Using the AWS CLI<a name="Users-groups-to-RGs-CLI"></a>

 The following AWS CLI operation creates a replication group with encryption in transit \(TLS\) enabled and the user\-group\-ids parameter with the value `my-user-group-id`\. Replace the subnet group `sng-test` with a subnet group that exists\.

**Key Parameters**
+ **\-\-engine** – Must be `redis`\.
+ **\-\-engine\-version** – Must be 6\.x or later\.
+ **\-\-transit\-encryption\-enabled** – Required for authentication and HIPAA eligibility\.
+ **\-\-user\-group\-ids** – This value provides user groups comprised of users with specified access permissions for the cluster\.
+ **\-\-cache\-subnet\-group** – Required for HIPAA eligibility\.

For Linux, macOS, or Unix:

```
aws elasticache create-replication-group \
    --replication-group-id "new-replication-group" \
    --replication-group-description "new-replication-group" \
    --engine "redis" \
    --engine-version "6.x" \
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
    --engine-version "6.x" ^
    --cache-node-type cache.m5.large ^
    --transit-encryption-enabled ^
    --user-group-ids "new-group-1" ^
    --cache-subnet-group "cache-subnet-group"
```

This will return the following response:

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
        "UserGroupIds": ["new-group-1"]
        "CacheNodeType": "cache.m5.large",
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

This will return the following response \(abbreviated\):

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

Note the `PendingChanges` in the response\. Any modifications made to a replication group are updated asynchronously\. You can monitor this progress by viewing the events\. For more information, see [Viewing ElastiCache Events](ECEvents.Viewing.md)

#### Migrating from Redis AUTH to RBAC<a name="Migrate-From-RBAC-to-Auth"></a>

If you are [Authenticating Users with the Redis AUTH Command](auth.md) and want to migrate to using RBAC, use the following examples:

**Migrating from Redis AUTH to RBAC using the AWS CLI**

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

**Migrating from Redis AUTH to RBAC using the AWS Management Console**

****

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region where the cluster that you want to modify is located\.

1. In the navigation pane, choose the engine running on the cluster that you want to modify\.

   A list of the chosen engine's clusters appears\.

1. In the list of clusters, for the cluster that you want to modify, choose its name\. 

1. Choose **Actions** and then choose **Modify**\. 

   The **Modify Cluster** window appears\.

1. In the **Modify Cluster** window, choose the **Access Control Option** dropdown and switch the value from **Redis AUTH Default User** to **User Group Access Control List**\.

1.  Under **User Group Access Control List**, select a user group\. 

1. Choose **Modify**\.

### Migrating from RBAC to Redis AUTH<a name="Migrate-From-RBAC-to-AUTH-1"></a>

If you are using RBAC and want to migrate to [Authenticating Users with the Redis AUTH Command](auth.md), use the following examples:

**Migrating from RBAC to Redis AUTH using the AWS CLI**

For Linux, macOS, or Unix:

```
aws elasticache modify-replication-group \
    --replication-group-id test \
    --remove-user-groups \
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

**Migrating from RBAC to Redis AUTH using the AWS Management Console**

****

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. From the list in the upper\-right corner, choose the AWS Region where the cluster that you want to modify is located\.

1. In the navigation pane, choose the engine running on the cluster that you want to modify\.

   A list of the chosen engine's clusters appears\.

1. In the list of clusters, for the cluster that you want to modify, choose its name\. 

1. Choose **Actions** and then choose **Modify**\. 

   The **Modify Cluster** window appears\.

1. In the **Modify Cluster** window, choose the **Access Control Option** dropdown and switch the value from **User Group Access Control List** to **Redis AUTH Default User**\.

1.  Under **AUTH token**, accept either **No change**, rotate an existing token or set a new token\. 

1. Choose **Modify**\.