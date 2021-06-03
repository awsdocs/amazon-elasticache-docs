# Accessing your cluster<a name="accessing-elasticache"></a>

Your Amazon ElastiCache instances are designed to be accessed through an Amazon EC2 instance\.

If you launched your ElastiCache instance in an Amazon Virtual Private Cloud \(Amazon VPC\), you can access your ElastiCache instance from an Amazon EC2 instance in the same Amazon VPC\. Or, by using VPC peering, you can access your ElastiCache instance from an Amazon EC2 in a different Amazon VPC\.

If you launched your ElastiCache instance in EC2 Classic, you allow the EC2 instance to access your cluster by granting the Amazon EC2 security group associated with the instance access to your cache security group\. By default, access to a cluster is restricted to the account that launched the cluster\.

**Topics**
+ [Determine the cluster's platform](#authorize-access-vpc-or-classic)
+ [Grant access to your cluster](#grant-access)

## Determine the cluster's platform<a name="authorize-access-vpc-or-classic"></a>

Before you continue, determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#detecting-platform)\.

### Determining Your Cluster's Platform using the ElastiCache Console<a name="authorize-access-vpc-or-classic-console"></a>

The following procedure uses the ElastiCache console to determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

**To determine a cluster's platform using the ElastiCache console**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of your clusters running the Memcached engine, in the left navigation pane, choose **Memcached**\.

1. In the list of clusters, expand the cluster you want to authorize access to by choosing the box to the left of the cluster name\.

1. Locate **Subnet group:**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/images/ElastiCache-SubnetGroup.png)
   + If the **Subnet group** has a name, as shown here, you launched your cluster in EC2\-VPC and should continue at [You launched your cluster into EC2\-VPC](#authorize-access-vpc)\.
   + If there is a dash \(\-\) instead of a **Subnet group** name, you launched your cluster in EC2\-Classic and should continue at [You launched your cluster running in EC2\-Classic](#authorize-access-ec2-classic)\.

For more information, see [Detecting Your Supported Platforms and Whether You Have a Default VPC](https://docs.aws.amazon.com/vpc/latest/userguide/default-vpc.html#detecting-platform)\.

### Determining Your Clusters Platform using the AWS CLI<a name="authorize-access-vpc-or-classic-cli"></a>

The following procedure uses the AWS CLI to determine whether you launched your cluster into EC2\-VPC or EC2\-Classic\.

**To determine a cluster's platform using the AWS CLI**

1. Open a command window\.

1. At the command prompt, run the following command\.

   For Linux, OS X, or Unix:

   ```
   aws elasticache describe-cache-clusters \
   --show-cache-cluster-details \
   --cache-cluster-id my-cluster
   ```

   For Windows:

   ```
   aws elasticache describe-cache-clusters ^
   --show-cache-cluster-details ^
   --cache-cluster-id my-cluster
   ```

   JSON output from this command will look something like this\. Some of the output is omitted to save space\.

   ```
   {
   "CacheClusters": [
       {
           "Engine": "memcached", 
            
           "AuthTokenEnabled": false, 
           "CacheParameterGroup": {
               "CacheNodeIdsToReboot": [], 
               "CacheParameterGroupName": "default.memcached1.4",
               
               "ParameterApplyStatus": "in-sync"
           }, 
           "CacheClusterId": "my-cluster-001", 
           "CacheSecurityGroups": [], 
           "NumCacheNodes": 1, 
           "AtRestEncryptionEnabled": false, 
           "CacheClusterCreateTime": "2018-01-16T20:09:34.449Z", 
           "ReplicationGroupId": "my-cluster", 
           "AutoMinorVersionUpgrade": true, 
           "CacheClusterStatus": "available", 
           "PreferredAvailabilityZone": "us-east-2a", 
           "ClientDownloadLandingPage": "https://console.aws.amazon.com/elasticache/home#client-download:", 
           "SecurityGroups": [
               {
                   "Status": "active", 
                   "SecurityGroupId": "sg-e8c03081"
               }
           ], 
           "TransitEncryptionEnabled": false, 
           "CacheSubnetGroupName": "default", 
           "EngineVersion": "3.2.10", 
           "PendingModifiedValues": {}, 
           "PreferredMaintenanceWindow": "sat:05:30-sat:06:30", 
           "CacheNodeType": "cache.t2.medium"
       }
   ]
   }
   ```
   + If there is a value for `CacheSubnetGroupName`, you launched your cluster in EC2\-VPC and should continue at [You launched your cluster into EC2\-VPC](#authorize-access-vpc)\.
   + If there is no value for `CacheSubnetGroupName`, you launched your cluster in EC2\-Classic and should continue at [You launched your cluster running in EC2\-Classic](#authorize-access-ec2-classic)\.

## Grant access to your cluster<a name="grant-access"></a>

### You launched your cluster into EC2\-VPC<a name="authorize-access-vpc"></a>

If you launched your cluster into an Amazon Virtual Private Cloud \(Amazon VPC\), you can connect to your ElastiCache cluster only from an Amazon EC2 instance that is running in the same Amazon VPC\. In this case, you will need to grant network ingress to the cluster\.

**Note**  
If your are using *Local Zones*, make sure you have enabled it\. For more information, see [Enable Local Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#opt-in-local-zone)\. By doing so, your VPC is extended to that Local Zone and your VPC will treat the subnet as any subnet in any other Availability Zone and relevant gateways, route tables and other security group considerations\. will be automatically adjusted\.

**To grant network ingress from an Amazon VPC security group to a cluster**

1. Sign in to the AWS Management Console and open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. In the navigation pane, under **Network & Security**, choose **Security Groups**\.

1. From the list of security groups, choose the security group for your Amazon VPC\. Unless you created a security group for ElastiCache use, this security group will be named *default*\.

1. Choose the **Inbound** tab, and then do the following:

   1. Choose **Edit**\.

   1. Choose **Add rule**\.

   1. In the **Type** column, choose **Custom TCP rule**\.

   1. In the **Port range** box, type the port number for your cluster node\. This number must be the same one that you specified when you launched the cluster\. The default port for Memcached is **11211** \.

   1. In the **Source** box, choose **Anywhere** which has the port range \(0\.0\.0\.0/0\) so that any Amazon EC2 instance that you launch within your Amazon VPC can connect to your ElastiCache nodes\.
**Important**  
Opening up the ElastiCache cluster to 0\.0\.0\.0/0 does not expose the cluster to the Internet because it has no public IP address and therefore cannot be accessed from outside the VPC\. However, the default security group may be applied to other Amazon EC2 instances in the customer’s account, and those instances may have a public IP address\. If they happen to be running something on the default port, then that service could be exposed unintentionally\. Therefore, we recommend creating a VPC Security Group that will be used exclusively by ElastiCache\. For more information, see [Custom Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html#creating-your-own-security-groups)\.

   1. Choose **Save**\.

When you launch an Amazon EC2 instance into your Amazon VPC, that instance will be able to connect to your ElastiCache cluster\.

### You launched your cluster running in EC2\-Classic<a name="authorize-access-ec2-classic"></a>

If you launched your cluster into EC2\-Classic, to allow an Amazon EC2 instance to access your cluster you will need to grant the Amazon EC2 security group associated with the instance access to your cache security group\.

**To grant an Amazon EC2 security group access to a cluster**

1. Sign in to the AWS Management Console and open the ElastiCache console at [ https://console\.aws\.amazon\.com/elasticache/](https://console.aws.amazon.com/elasticache/)\.

1. To see a list of security groups, from the left navigation pane, choose **Security Groups**\.
**Important**  
If **Security Groups** is not listed in the navigation pane, you launched your cluster in EC2\-VPC rather than EC2\-Classic and should follow the instructions at [You launched your cluster into EC2\-VPC](#authorize-access-vpc)\.

1. Choose the box to the left of **default** security group\.

1. From the list at the bottom of the screen, choose the **EC2 Security Group Name** you want to authorize\.

1. To authorize access, choose **Add**\.

   Amazon EC2 instances that are associated with the security group are now authorized to connect to your ElastiCache cluster\.

To revoke a security group's access, locate the security group in the list of authorized security groups, and then choose **Remove**\.

For more information on ElastiCache Security Groups, see [Security groups: EC2\-Classic](SecurityGroups.md)\.

### Accessing ElastiCache resources from outside AWS<a name="access-from-outside-aws"></a>

Elasticache is a service designed to be used internally to your VPC\. External access is discouraged due to the latency of Internet traffic and security concerns\. However, if external access to Elasticache is required for test or development purposes, it can be done through a VPN\.

Using the AWS Client VPN, you allow external access to your Elasticache nodes with the following benefits:
+ Restricted access to approved users or authentication keys;
+ Encrypted traffic between the VPN Client and the AWS VPN endpoint;
+ Limited access to specific subnets or nodes;
+ Easy revocation of access from users or authentication keys;
+ Audit connections;

The following procedures demonstrate how to:

**Topics**
+ [Create a certificate authority](#create-cert)
+ [Configuring AWS client VPN components](#configure-vpn-components)
+ [Configure the VPN client](#configure-vpn-client)

#### Create a certificate authority<a name="create-cert"></a>

It is possible to create a Certificate Authority \(CA\) using different techniques or tools\. We suggest the easy\-rsa utility, provided by the [OpenVPN](https://openvpn.net/community-resources/openvpn-project/) project\. Regardless of the option you choose, make sure to keep the keys secure\. The following procedure downloads the easy\-rsa scripts, creates the Certificate Authority and the keys to authenticate the first VPN client:
+ To create the initial certificates, open a terminal and do the following:
  + `git clone` [https://github\.com/OpenVPN/easy\-rsa](https://github.com/OpenVPN/easy-rsa)
  + `cd easy-rsa`
  + `./easyrsa3/easyrsa init-pki`
  + `./easyrsa3/easyrsa build-ca nopass`
  + `./easyrsa3/easyrsa build-server-full server nopass`
  + `./easyrsa3/easyrsa build-client-full client1.domain.tld nopass`

  A **pki** subdirectory containing the certificates will be created under **easy\-rsa**\.
+ Submit the server certificate to the AWS Certificate manager \(ACM\):
  + On the ACM console, select **Certificate Manager**\.
  + Select **Import Certificate**\.
  + Enter the public key certificate available in the `easy-rsa/pki/issued/server.crt` file in the **Certificate body** field\.
  + Paste the private key available in the `easy-rsa/pki/private/server.key` in the **Certificate private key** field\. Make sure to select all the lines between `BEGIN AND END PRIVATE KEY` \(including the `BEGIN` and `END` lines\)\.
  + Paste the CA public key available on the `easy-rsa/pki/ca.crt` file in the **Certificate chain** field\.
  + Select **Review and import**\.
  + Select **Import**\.

  To submit the server's certificates to ACM using the AWS CLI, run the following command: `aws acm import-certificate --certificate fileb://easy-rsa/pki/issued/server.crt --private-key file://easy-rsa/pki/private/server.key --certificate-chain file://easy-rsa/pki/ca.crt --region region`

  Note the Certificate ARN for future use\.

#### Configuring AWS client VPN components<a name="configure-vpn-components"></a>

**Using the AWS Console**

On the AWS console, select **Services** and then **VPC**\.

Under **Virtual Private Network**, select **Client VPN Endpoints** and do the following:

**Configuring AWS Client VPN components**
+ Select **Create Client VPN Endpoint**\.
+ Specify the following options:
  + **Client IPv4 CIDR**: use a private network with a netmask of at least /22 range\. Make sure that the selected subnet does not conflict with the VPC networks' addresses\. Example: 10\.0\.0\.0/22\.
  + In **Server certificate ARN**, select the ARN of the certificate previously imported\.
  + Select **Use mutual authentication**\.
  + In **Client certificate ARN**, select the ARN of the certificate previously imported\.
  + Select **Create Client VPN Endpoint**\.

**Using the AWS CLI**

Run the following command:

`aws ec2 create-client-vpn-endpoint --client-cidr-block "10.0.0.0/22" --server-certificate-arn arn:aws:acm:us-east-1:012345678912:certificate/0123abcd-ab12-01a0-123a-123456abcdef --authentication-options Type=certificate-authentication,,MutualAuthentication={ClientRootCertificateChainArn=arn:aws:acm:us-east-1:012345678912:certificate/123abcd-ab12-01a0-123a-123456abcdef} --connection-log-options Enabled=false `

Example output:

`"ClientVpnEndpointId": "cvpn-endpoint-0123456789abcdefg", "Status": { "Code": "pending-associate" }, "DnsName": "cvpn-endpoint-0123456789abcdefg.prod.clientvpn.us-east-1.amazonaws.com" } `

**Associate the target networks to the VPN endpoint**
+ Select the new VPN endpoint, and then select the **Associations** tab\.
+ Select **Associate** and specify the following options\.
  + **VPC**: Select the Elasticache Cluster's VPC\.
  + Select one of the Elasticache cluster's networks\. If in doubt, review the networks in the **Subnet Groups** on the Elasticache dashboard\.
  + Select **Associate**\. If necessary, repeat the steps for the remaining networks\.

**Using the AWS CLI**

Run the following command:

`aws ec2 associate-client-vpn-target-network --client-vpn-endpoint-id cvpn-endpoint-0123456789abcdefg --subnet-id subnet-0123456789abdcdef`

Example output:

`"Status": { "Code": "associating" }, "AssociationId": "cvpn-assoc-0123456789abdcdef" } `

**Review the VPN security group**

The VPN Enpoint will automatically adopt the VPC's default security group\. Check the inbound and outbound rules and confirm if the security group allows the traffic from the VPN network \(defined on the VPN Endpoint settings\) to the Elasticache networks on the service ports \(by default, 6379 for Redis and 11211 for Memcached\)\.

If you need to change the security group assigned to the VPN Endpoint, proceed as follows:
+ Select the current security group\.
+ Select **Apply Security Group**\.
+ Select the new Security Group\.

**Using the AWS CLI**

Run the following command:

`aws ec2 apply-security-groups-to-client-vpn-target-network --client-vpn-endpoint-id cvpn-endpoint-0123456789abcdefga  --vpc-id vpc-0123456789abdcdef --security-group-ids sg-0123456789abdcdef`

Example output:

`"SecurityGroupIds": [ "sg-0123456789abdcdef" ] } `

**Note**  
The ElastiCache security group also needs to allow traffic coming from the VPN clients\. The clients' addresses will be masked with the VPN Endpoint address, according to the VPC Network\. Therefore, consider the VPC network \(not the VPN Clients' network\) when creating the inbound rule on the Elasticache security group\.

**Authorize the VPN access to the destination networks**

On the **Authorization** tab, select **Authorize Ingress** and specify the following:
+ Destination network to enable access: Either use 0\.0\.0\.0/0 to allow access to any network \(including the Internet\) or restrict the the Elasticache networks/hosts\.
+ Under **Grant access to:**, select **Allow access to all users**\.
+ Select **Add Authorization Rules**\.

**Using the AWS CLI**

Run the following command:

`aws ec2 authorize-client-vpn-ingress --client-vpn-endpoint-id cvpn-endpoint-0123456789abcdefg --target-network-cidr 0.0.0.0/0 --authorize-all-groups`

Example output: 

`{ "Status": { "Code": "authorizing" } }`

**Allowing access to the Internet from the VPN clients**

If you need to browse the Internet through the VPN, you need to create an additional route\. Select the **Route Table** tab and then select **Create Route**:
+ Route destination: 0\.0\.0\.0/0
+ **Target VPC Subnet ID**: Select one of the associated subnets with access to the Internet\.
+ Select **Create Route**\.

**Using the AWS CLI**

Run the following command:

`aws ec2 create-client-vpn-route --client-vpn-endpoint-id cvpn-endpoint-0123456789abcdefg --destination-cidr-block 0.0.0.0/0 --target-vpc-subnet-id subnet-0123456789abdcdef`

Example output:

`{ "Status": { "Code": "creating" } } `

#### Configure the VPN client<a name="configure-vpn-client"></a>

On the AWS Client VPN Dashboard, select the VPN endpoint recently created and select **Download Client Configuration**\. Copy the configuration file, and the files `easy-rsa/pki/issued/client1.domain.tld.crt` and `easy-rsa/pki/private/client1.domain.tld.key`\. Edit the configuration file and change or add the following parameters:
+ cert: add a new line with the parameter cert pointing to the `client1.domain.tld.crt` file\. Use the full path to the file\. Example: `cert /home/user/.cert/client1.domain.tld.crt`
+ cert: key: add a new line with the parameter key pointing to the `client1.domain.tld.key` file\. Use the full path to the file\. Example: `key /home/user/.cert/client1.domain.tld.key`

Establish the VPN connection with the command: `sudo openvpn --config downloaded-client-config.ovpn`

**Revoking access**

If you need to invalidate the access from a particular client key, the key needs to be revoked in the CA\. Then submit the revocation list to AWS Client VPN\.

Revoking the key with easy\-rsa: 
+ `cd easy-rsa`
+ `./easyrsa3/easyrsa revoke client1.domain.tld`
+ Enter "yes" to continue, or any other input to abort\.

  `Continue with revocation: `yes` ... * `./easyrsa3/easyrsa gen-crl`
+ An updated CRL has been created\. CRL file: `/home/user/easy-rsa/pki/crl.pem` 

Importing the revocation list to the AWS Client VPN:
+ On the AWS Management Console, select **Services** and then **VPC**\.
+ Select **Client VPN Endpoints**\.
+ Select the Client VPN Endpoint and then select **Actions** \-> **Import Client Certificate CRL**\.
+ Paste the contents of the `crl.pem` file\. 

**Using the AWS CLI**

Run the following command:

`aws ec2 import-client-vpn-client-certificate-revocation-list --certificate-revocation-list file://./easy-rsa/pki/crl.pem --client-vpn-endpoint-id cvpn-endpoint-0123456789abcdefg `

Example output:

`Example output: { "Return": true } `