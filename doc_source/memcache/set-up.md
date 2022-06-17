# Setting up<a name="set-up"></a>

Following, you can find topics that describe the one\-time actions you must take to start using ElastiCache\.

**Topics**
+ [Create your AWS account](#elasticache-create-aws-account)
+ [Getting an AWS Access Key](#elasticache-set-up-access-key)
+ [Configuring Your Credentials](#elasticache-configure-credentials)
+ [Downloading and Configuring the AWS CLI](#elasticache-install-configure-cli)
+ [Set up your permissions \(new ElastiCache users only\)](#elasticache-set-up-permissions)

## Create your AWS account<a name="elasticache-create-aws-account"></a>

To use Amazon ElastiCache, you must have an active AWS account and permissions to access ElastiCache and other AWS resources\.

If you don't already have an AWS account, create one now\. AWS accounts are free\. You are not charged for signing up for an AWS service, only for using AWS services\.

**To create an AWS account**

1. Open [https://portal\.aws\.amazon\.com/billing/signup](https://portal.aws.amazon.com/billing/signup)\.

1. Follow the online instructions\.

   Part of the sign\-up procedure involves receiving a phone call and entering a verification code on the phone keypad\.

## Getting an AWS Access Key<a name="elasticache-set-up-access-key"></a>

Before you can access ElastiCache programmatically or through the AWS Command Line Interface \(AWS CLI\), you must have an AWS access key\. You don't need an access key if you plan to use the ElastiCache console only\. Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. As a best practice, do not use the AWS account root user access keys for any task where it's not required\. Instead, create a new administrator IAM user with access keys for yourself\. The only time that you can view or download the secret access key is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions Required to Access IAM Resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

**To create access keys for an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. Choose the name of the user whose access keys you want to create, and then choose the **Security credentials** tab\.

1. In the **Access keys** section, choose **Create access key**\.

1. To view the new access key pair, choose Show\. You will not have access to the secret access key again after this dialog box closes\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\. You will not have access to the secret access key again after this dialog box closes\.

1. Keep the keys confidential in order to protect your AWS account and never email them\. Do not share them outside your organization, even if an inquiry appears to come from Amazon or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

1. After you download the \.csv file, choose **Close**\. When you create an access key, the key pair is active by default, and you can use the pair right away\.

**Related topics:**
+ [What is IAM](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*\.
+ [AWS Security Credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference*\.

## Configuring Your Credentials<a name="elasticache-configure-credentials"></a>

Before you can access ElastiCache programatically or through the AWS CLI, you must configure your credentials to enable authorization for your applications\.

 There are several ways to do this\. For example, you can manually create the credentials file to store your access key ID and secret access key\. You also can use the `aws configure `command of the AWS CLI to automatically create the file\. Alternatively, you can use environment variables\. For more information about configuring your credentials, see the programming\-specific AWS SDK developer guide at [Tools to Build on AWS](https://aws.amazon.com/tools/)\.

## Downloading and Configuring the AWS CLI<a name="elasticache-install-configure-cli"></a>

The AWS CLI is available at [http://aws\.amazon\.com/cli](http://aws.amazon.com/cli)\. It runs on Windows, MacOS and Linux\. After you download the AWS CLI, follow these steps to install and configure it:

1. Go to the [AWS Command Line Interface User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Follow the instructions for [Installing the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html) and [Configuring the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)\.

## Set up your permissions \(new ElastiCache users only\)<a name="elasticache-set-up-permissions"></a>

Amazon ElastiCache creates and uses service\-linked roles to provision resources and access other AWS resources and services on your behalf\. For ElastiCache to create a service\-linked role for you, use the AWS\-managed policy named `AmazonElastiCacheFullAccess`\. This role comes preprovisioned with permission that the service requires to create a service\-linked role on your behalf\.

You might decide not to use the default policy and instead to use a custom\-managed policy\. In this case, make sure that you have either permissions to call `iam:createServiceLinkedRole` or that you have created the ElastiCache service\-linked role\. 

For more information, see the following:
+ [Creating a New Policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create.html) \(IAM\)
+ [AWS\-managed \(predefined\) policies for Amazon ElastiCache](IAM.IdentityBasedPolicies.md#IAM.IdentityBasedPolicies.PredefinedPolicies)
+ [Using Service\-Linked Roles for Amazon ElastiCache](using-service-linked-roles.md)