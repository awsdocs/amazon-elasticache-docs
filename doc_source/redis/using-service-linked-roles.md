# Using Service\-Linked Roles for Amazon ElastiCache<a name="using-service-linked-roles"></a>

Amazon ElastiCache uses AWS Identity and Access Management \(IAM\) [service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_terms-and-concepts.html#iam-term-service-linked-role)\. A service\-linked role is a unique type of IAM role that is linked directly to an AWS service, such as Amazon ElastiCache\. Amazon ElastiCache service\-linked roles are predefined by Amazon ElastiCache\. They include all the permissions that the service requires to call AWS services on behalf of your clusters\. 

A service\-linked role makes setting up Amazon ElastiCache easier because you don’t have to manually add the necessary permissions\. The roles already exist within your AWS account but are linked to Amazon ElastiCache use cases and have predefined permissions\. Only Amazon ElastiCache can assume these roles, and only these roles can use the predefined permissions policy\. You can delete the roles only after first deleting their related resources\. This protects your Amazon ElastiCache resources because you can't inadvertently remove necessary permissions to access the resources\.

For information about other services that support service\-linked roles, see [AWS Services That Work with IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html) and look for the services that have **Yes **in the **Service\-Linked Role** column\. Choose a **Yes** with a link to view the service\-linked role documentation for that service\.

**Contents**
+ [Service\-Linked Role Permissions](#service-linked-role-permissions)
+ [Creating a Service\-Linked Role \(IAM\)](#create-service-linked-role-iam)
  + [Using the IAM Console](#create-service-linked-role-iam-console)
  + [Using the IAM CLI](#create-service-linked-role-iam-cli)
  + [Using the IAM API](#create-service-linked-role-iam-api)
+ [Editing a Service\-Linked Role Description](#edit-service-linked-role)
  + [Using the IAM Console](#edit-service-linked-role-iam-console)
  + [Using the IAM CLI](#edit-service-linked-role-iam-cli)
  + [Using the IAM API](#edit-service-linked-role-iam-api)
+ [Deleting a Service\-Linked Role for Amazon ElastiCache](#delete-service-linked-role)
  + [Cleaning Up a Service\-Linked Role](#service-linked-role-review-before-delete)
  + [Deleting a Service\-Linked Role \(IAM Console\)](#delete-service-linked-role-iam-console)
  + [Deleting a Service\-Linked Role \(IAM CLI\)](#delete-service-linked-role-iam-cli)
  + [Deleting a Service\-Linked Role \(IAM API\)](#delete-service-linked-role-iam-api)

## Service\-Linked Role Permissions for Amazon ElastiCache<a name="service-linked-role-permissions"></a>

Amazon ElastiCache uses the service\-linked role named **AWSServiceRoleForElastiCache** – This policy allows ElastiCache to manage AWS resources on your behalf as necessary for managing your cache\.

The AWSServiceRoleForElastiCache service\-linked role permissions policy allows Amazon ElastiCache to complete the following actions on the specified resources:

```
Permission policy:
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:AuthorizeSecurityGroupIngress",
        "ec2:CreateNetworkInterface",
        "ec2:CreateSecurityGroup",
        "ec2:DeleteNetworkInterface",
        "ec2:DeleteSecurityGroup",
        "ec2:DescribeAvailabilityZones",
        "ec2:DescribeNetworkInterfaces",
        "ec2:DescribeSecurityGroups",
        "ec2:DescribeSubnets",
        "ec2:DescribeVpcs",
        "ec2:ModifyNetworkInterfaceAttribute",
        "ec2:RevokeSecurityGroupIngress",
      ],
      "Resource": "*"
    }
  ]
}
```

**To allow an IAM entity to create AWSServiceRoleForElastiCache service\-linked roles**

Add the following policy statement to the permissions for that IAM entity:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:CreateServiceLinkedRole",
        "iam:PutRolePolicy"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/elasticache.amazonaws.com/AWSServiceRoleForElastiCache*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "elasticache.amazonaws.com"}}
}
```

**To allow an IAM entity to delete AWSServiceRoleForElastiCache service\-linked roles**

Add the following policy statement to the permissions for that IAM entity:

```
{
    "Effect": "Allow",
    "Action": [
        "iam:DeleteServiceLinkedRole",
        "iam:GetServiceLinkedRoleDeletionStatus"
    ],
    "Resource": "arn:aws:iam::*:role/aws-service-role/elasticache.amazonaws.com/AWSServiceRoleForElastiCache*",
    "Condition": {"StringLike": {"iam:AWSServiceName": "elasticache.amazonaws.com"}}
}
```

Alternatively, you can use an AWS managed policy to provide full access to Amazon ElastiCache\.

## Creating a Service\-Linked Role \(IAM\)<a name="create-service-linked-role-iam"></a>

You can create a service\-linked role using the IAM console, CLI, or API\.

### Creating a Service\-Linked Role \(IAM Console\)<a name="create-service-linked-role-iam-console"></a>

You can use the IAM console to create a service\-linked role\.

**To create a service\-linked role \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then choose **Create new role**\.

1. Expand the **AWS service\-linked role** section, and then select the service that you want to allow to assume this new service\-linked role\.

1. Next to the **AWSServiceRoleForElastiCache** service\-linked role, choose **Select**\.

1. For **Role name**, type a suffix to add to the service\-linked role default name\. This suffix helps you identify the purpose of this role\. Role names must be unique within your AWS account\. They are not distinguished by case\. For example, you cannot create roles named both **<service\-linked\-role\-name>\_SAMPLE** and **<service\-linked\-role\-name>\_sample**\. Because various entities might reference the role, you cannot edit the name of the role after it has been created\.

1. \(Optional\) For **Role description**, edit the description for the new service\-linked role\.

1. Review the role and then choose **Create role**\.

### Creating a Service\-Linked Role \(IAM CLI\)<a name="create-service-linked-role-iam-cli"></a>

You can use IAM operations from the AWS Command Line Interface to create a service\-linked role\. This role can include the trust policy and inline policies that the service needs to assume the role\.

**To create a service\-linked role \(CLI\)**

Use the following operation:

```
$ aws iam [create\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/create-service-linked-role.html) --aws-service-name elasticache.amazonaws.com
```

### Creating a Service\-Linked Role \(IAM API\)<a name="create-service-linked-role-iam-api"></a>

You can use the IAM API to create a service\-linked role\. This role can contain the trust policy and inline policies that the service needs to assume the role\.

**To create a service\-linked role \(API\)**

Use the [CreateServiceLinkedRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_CreateServiceLinkedRole.html) API call\. In the request, specify a service name of `elasticache.amazonaws.com`\. 

## Editing the Description of a Service\-Linked Role for Amazon ElastiCache<a name="edit-service-linked-role"></a>

Amazon ElastiCache does not allow you to edit the AWSServiceRoleForElastiCache service\-linked role\. After you create a service\-linked role, you cannot change the name of the role because various entities might reference the role\. However, you can edit the description of the role using IAM\.

### Editing a Service\-Linked Role Description \(IAM Console\)<a name="edit-service-linked-role-iam-console"></a>

You can use the IAM console to edit a service\-linked role description\.

**To edit the description of a service\-linked role \(console\)**

1. In the navigation pane of the IAM console, choose **Roles**\.

1. Choose the name of the role to modify\.

1. To the far right of **Role description**, choose **Edit**\. 

1. Enter a new description in the box and choose **Save**\.

### Editing a Service\-Linked Role Description \(IAM CLI\)<a name="edit-service-linked-role-iam-cli"></a>

You can use IAM operations from the AWS Command Line Interface to edit a service\-linked role description\.

**To change the description of a service\-linked role \(CLI\)**

1. \(Optional\) To view the current description for a role, use the AWS CLI for IAM operation `[get\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/get-role.html)`\.  
**Example**  

   ```
   $ aws iam [get\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/get-role.html) --role-name AWSServiceRoleForElastiCache
   ```

   Use the role name, not the ARN, to refer to roles with the CLI operations\. For example, if a role has the following ARN: `arn:aws:iam::123456789012:role/myrole`, refer to the role as **myrole**\.

1. To update a service\-linked role's description, use the AWS CLI for IAM operation `[update\-role\-description](https://docs.aws.amazon.com/cli/latest/reference/iam/update-role-description.html)`\.

   For Linux, macOS, or Unix:

   ```
   $ aws iam [update\-role\-description](https://docs.aws.amazon.com/cli/latest/reference/iam/update-role-description.html) \
       --role-name AWSServiceRoleForElastiCache \
       --description "new description"
   ```

   For Windows:

   ```
   $ aws iam [update\-role\-description](https://docs.aws.amazon.com/cli/latest/reference/iam/update-role-description.html) ^
       --role-name AWSServiceRoleForElastiCache ^
       --description "new description"
   ```

### Editing a Service\-Linked Role Description \(IAM API\)<a name="edit-service-linked-role-iam-api"></a>

You can use the IAM API to edit a service\-linked role description\.

**To change the description of a service\-linked role \(API\)**

1. \(Optional\) To view the current description for a role, use the IAM API operation [GetRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetRole.html)\.  
**Example**  

   ```
   https://iam.amazonaws.com/
      ?Action=[GetRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetRole.html)
      &RoleName=AWSServiceRoleForElastiCache
      &Version=2010-05-08
      &AUTHPARAMS
   ```

1. To update a role's description, use the IAM API operation [UpdateRoleDescription](https://docs.aws.amazon.com/IAM/latest/APIReference/API_UpdateRoleDescription.html)\.  
**Example**  

   ```
   https://iam.amazonaws.com/
      ?Action=[UpdateRoleDescription](https://docs.aws.amazon.com/IAM/latest/APIReference/API_UpdateRoleDescription.html)
      &RoleName=AWSServiceRoleForElastiCache
      &Version=2010-05-08
      &Description="New description"
   ```

## Deleting a Service\-Linked Role for Amazon ElastiCache<a name="delete-service-linked-role"></a>

If you no longer need to use a feature or service that requires a service\-linked role, we recommend that you delete that role\. That way you don’t have an unused entity that is not actively monitored or maintained\. However, you must clean up your service\-linked role before you can delete it\.

Amazon ElastiCache does not delete the service\-linked role for you\.

### Cleaning Up a Service\-Linked Role<a name="service-linked-role-review-before-delete"></a>

Before you can use IAM to delete a service\-linked role, first confirm that the role has no resources \(clusters or replication groups\) associated with it\.

**To check whether the service\-linked role has an active session in the IAM console**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then choose the name \(not the check box\) of the AWSServiceRoleForElastiCache role\.

1. On the **Summary** page for the selected role, choose the **Access Advisor** tab\.

1. On the **Access Advisor** tab, review recent activity for the service\-linked role\.

**To delete Amazon ElastiCache resources that require AWSServiceRoleForElastiCache \(console\)**
+ To delete a cluster, see the following:
  + [Using the AWS Management Console](Clusters.Delete.md#Clusters.Delete.CON)
  + [Using the AWS CLI](Clusters.Delete.md#Clusters.Delete.CLI)
  + [Using the ElastiCache API](Clusters.Delete.md#Clusters.Delete.API)
+ To delete a replication group, see the following:
  + [Deleting a Replication Group \(Console\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.CON)
  + [Deleting a Replication Group \(AWS CLI\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.CLI)
  + [Deleting a replication group \(ElastiCache API\)](Replication.DeletingRepGroup.md#Replication.DeletingRepGroup.API)

### Deleting a Service\-Linked Role \(IAM Console\)<a name="delete-service-linked-role-iam-console"></a>

You can use the IAM console to delete a service\-linked role\.

**To delete a service\-linked role \(console\)**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane of the IAM console, choose **Roles**\. Then select the check box next to the role name that you want to delete, not the name or row itself\. 

1. For **Role actions** at the top of the page, choose **Delete role**\.

1. In the confirmation dialog box, review the service last accessed data, which shows when each of the selected roles last accessed an AWS service\. This helps you to confirm whether the role is currently active\. If you want to proceed, choose **Yes, Delete** to submit the service\-linked role for deletion\.

1. Watch the IAM console notifications to monitor the progress of the service\-linked role deletion\. Because the IAM service\-linked role deletion is asynchronous, after you submit the role for deletion, the deletion task can succeed or fail\. If the task fails, you can choose **View details** or **View Resources** from the notifications to learn why the deletion failed\.

### Deleting a Service\-Linked Role \(IAM CLI\)<a name="delete-service-linked-role-iam-cli"></a>

You can use IAM operations from the AWS Command Line Interface to delete a service\-linked role\.

**To delete a service\-linked role \(CLI\)**

1. If you don't know the name of the service\-linked role that you want to delete, enter the following command\. This command lists the roles and their Amazon Resource Names \(ARNs\) in your account\.

   ```
   $ aws iam [get\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/get-role.html) --role-name role-name
   ```

   Use the role name, not the ARN, to refer to roles with the CLI operations\. For example, if a role has the ARN `arn:aws:iam::123456789012:role/myrole`, you refer to the role as **myrole**\.

1. Because a service\-linked role cannot be deleted if it is being used or has associated resources, you must submit a deletion request\. That request can be denied if these conditions are not met\. You must capture the `deletion-task-id` from the response to check the status of the deletion task\. Enter the following to submit a service\-linked role deletion request\.

   ```
   $ aws iam [delete\-service\-linked\-role](https://docs.aws.amazon.com/cli/latest/reference/iam/delete-service-linked-role.html) --role-name role-name
   ```

1. Enter the following to check the status of the deletion task\.

   ```
   $ aws iam [get\-service\-linked\-role\-deletion\-status](https://docs.aws.amazon.com/cli/latest/reference/iam/get-service-linked-role-deletion-status.html) --deletion-task-id deletion-task-id
   ```

   The status of the deletion task can be `NOT_STARTED`, `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. If the deletion fails, the call returns the reason that it failed so that you can troubleshoot\.

### Deleting a Service\-Linked Role \(IAM API\)<a name="delete-service-linked-role-iam-api"></a>

You can use the IAM API to delete a service\-linked role\.

**To delete a service\-linked role \(API\)**

1. To submit a deletion request for a service\-linked roll, call [DeleteServiceLinkedRole](https://docs.aws.amazon.com/IAM/latest/APIReference/API_DeleteServiceLinkedRole.html)\. In the request, specify a role name\.

   Because a service\-linked role cannot be deleted if it is being used or has associated resources, you must submit a deletion request\. That request can be denied if these conditions are not met\. You must capture the `DeletionTaskId` from the response to check the status of the deletion task\.

1. To check the status of the deletion, call [GetServiceLinkedRoleDeletionStatus](https://docs.aws.amazon.com/IAM/latest/APIReference/API_GetServiceLinkedRoleDeletionStatus.html)\. In the request, specify the `DeletionTaskId`\.

   The status of the deletion task can be `NOT_STARTED`, `IN_PROGRESS`, `SUCCEEDED`, or `FAILED`\. If the deletion fails, the call returns the reason that it failed so that you can troubleshoot\.