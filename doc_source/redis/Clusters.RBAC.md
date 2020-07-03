# Using Role Based Access Control \(RBAC\)<a name="Clusters.RBAC"></a>

Amazon ElastiCache for Redis 6\.0\.5 now provides you the ability to create and manage users and user groups that can be used to set up Role Based Access Control \(RBAC\) for Redis commands\. You can now establish security boundaries between applications using the same Redis cluster\(s\), preventing those applications from accessing each other’s data\. You can also take advantage of granular access control and authorization to create user groups, such as administrators, readers etc\. 

**Topics**
+ [User Management Using the AWS Management Console](#Users-console)
+ [User Management Using the AWS CLI](#Users-cli)
+ [User Group Management Using the AWS Management Console](#User-Groups-console)
+ [User Group Management Using the AWS CLI](#User-Groups-cli)

## User Management Using the AWS Management Console<a name="Users-console"></a>

Sign in to the AWS Management Console and open the Amazon ElastiCache console at [https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

On the Amazon ElastiCache Dashboard, select **User Management**\. The following operations are available:
+ **Create User** – When creating a user, you enter a User ID, User Name, Password and Access String, which is the permission level on operations the user is allowed\. Redis automatically includes a *Default* user account whose **Access String** is `on nopass ~* +@all`\. This is broken down as following:
  + `on` – User is active\.
  + `nopass` – Password is not required\.
  + `~*` – Access to all available keys\.
  + `+@all` – Access to all available commands\.

  You can modify the *Default* user access settings or set specific settings for any new user account you create\. For more information on Redis Access Control Lists, see [ACL](https://redis.io/topics/acl)\.
+ **Modify User** – Allows you to update a user's password or change its access permissions\.
+ **Delete User** – The user account will be removed from any User Management Groups to which it belongs\.

## User Management Using the AWS CLI<a name="Users-cli"></a>
+ **Create User** – When creating a user, you enter a User ID, User Name, Password and Access String, which is the permission level on operations the user is allowed\. Redis automatically includes a *Default* user account whose **Access String** is `on nopass ~* +@all`\. This is broken down as following:
  + `on` – User is active\.
  + `nopass` – Password is not required\.
  + `~*` – Access to all available keys\.
  + `+@all` – Access to all available commands\.

  You can modify the *Default* user access settings or set specific settings for any new user account you create\. For more information on Redis Access Control Lists, see [ACL](https://redis.io/topics/acl)\.

  The following is an example:

  ```
  aws elasticache create-user \
   --user-id "user-123" \
   --user-name "default" \
   --engine "REDIS" \
   --passwords "password123" \
   --access-string "+get ~keys*"
  ```

  This will return the following response:

  ```
  {
    "UserId": "user-123",
    "UserName": "default",
    "Status": "CREATING",
    "Engine": "REDIS",
    "AccessString": "+get ~keys*",
    "UserGroupIds": []
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:user:user-123",
    "Authentication": {
      "Type": "password",
      "PasswordCount": 1
    } 
  }
  ```
+ **Modify User** – Allows you to update a user's password or change its access permissions\.

  The following is an example:

  ```
  aws elasticache modify-user \
    --user-id "user-123" \
    --access-string "~objects:* ~items:* ~public:*" \
    --no-password-required
  ```

  This will return the following response:

  ```
  {
    "UserId": "user-123",
    "UserName": "user123",
    "Status": "MODIFYING",
    "Engine": "REDIS",
    "AccessString": "~objects:* ~items:* ~public:*",
    "UserGroupIds": ["user-group-1", "user-group-2"],
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:user:user-123",
    "Authentication": {
      "Type": "no-password"
    }
  }
  ```
+ **Delete User** – The user account will be removed from any User Management Groups to which it belongs\.

  The following is an example:

  ```
  aws elasticache delete-user \
    --user-id "user-123"
  ```

  This will return the following response:

  ```
  {
    "UserId": "user-123",
    "UserName": "foo",
    "Status": "deleting",
    "Engine": "redis",
    "AccessString": "~objects:* ~items:* ~public:* +set +get",
    "UserGroupIds": ["user-group-1", "user-group-2"]
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:user:user-123",
    "Authentication": {
       "Type": "password",
       "PasswordCount": 1
    } 
  }
  ```

## User Group Management Using the AWS Management Console<a name="User-Groups-console"></a>

On the Amazon ElastiCache Dashboard, select **User Management**\. The following operations are available:
+ **Create** – When you create a user group, you add users for access control to clusters\. For example, you could create an **Admin** user group for users who have administrative roles on a cluster\.
+ **Delete** – Use this to delete a user group\. Note that the user group itself, not the users belonging to the group, will be deleted\.

## User Group Management Using the AWS CLI<a name="User-Groups-cli"></a>
+ **Create** – When you create a user group, you add users for access control to clusters\. For example, you could create an `Admin` user group for users who have administrative roles on a cluster\. 

  The following is an example:

  ```
  aws elasticache create-user-group \
    --user-group-id "new-group-1" \
    --engine "REDIS" \
    --user-ids "user-123"
  ```

  This will return the following response:

  ```
  {
    "UserGroupId": "new-group-1",
    "Status": "CREATING",
    "Engine": "REDIS",
    "UserIds": ["user-123"],
    "ARN": "arn:aws:elasticache:us-east-1:0123456789:usergroup:new-group-1" 
  }
  ```
+ **Delete** – Use this to delete a user group\. Note that the user group itself, not the users belonging to the group, will be deleted\.