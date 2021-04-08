# ElastiCache for Redis compliance<a name="elasticache-compliance"></a>

In this section, you can find the compliance requirements and controls offered when using Amazon ElastiCache for Redis\. 

**Topics**
+ [Self\-service security updates for compliance](#elasticache-compliance-self-service)
+ [ElastiCache for Redis FedRAMP compliance](#elasticache-compliance-fedramp)
+ [HIPAA eligibility](#elasticache-compliance-hipaa)
+ [ElastiCache for Redis PCI DSS compliance](#elasticache-compliance-pci)
+ [Create and seed a new compliant cluster](#elasticache-compliance-create-cluster)
+ [More information](#elasticache-compliance-see-also)

## Self\-service security updates for compliance<a name="elasticache-compliance-self-service"></a>

ElastiCache offers a self\-service software update feature called **Service Updates** by using the console, API, and CLI\. Using this feature, you can manage security updates on your Redis clusters on\-demand and in real\-time\. This feature allows you to control when you update Redis clusters with the latest required security fixes, minimizing the impact on your business\.

Security updates are released by using the **Service Updates** feature\. They are specified by the **Update Type** field of value **security update**\. The Service Update has corresponding **Severity** and **Recommended Apply by Date** fields\. In order to maintain compliance of your Redis clusters, you must apply the available updates by the **Recommended Apply by Date**\. The field **SLA Met** reflects your Redis cluster’s compliance status\. 

**Note**  
If you do not apply the Service Update by the recommended date or when the Service Update expires, ElastiCache will not take any action to apply the update on your behalf\.  
You are notified of the Service Updates applicable to your Redis clusters by an announcement on the Redis console, email, Amazon SNS, CloudWatch events and Personal Health Dashboard\. For more information on Self\-Service Maintenance see [Self\-service updates in Amazon ElastiCache](Self-Service-Updates.md)\.   
   
CloudWatch events and Personal Health Dashboard are not supported in the following regions:  
us\-gov\-west\-1 
us\-gov\-east\-1
cn\-north\-1
cn\-northwest\-1

## ElastiCache for Redis FedRAMP compliance<a name="elasticache-compliance-fedramp"></a>

The AWS FedRAMP Compliance program includes Amazon ElastiCache for Redis as a FedRAMP\-authorized service\. If you are a federal or commercial customer, you can use the service to process and store sensitive workloads in AWS US East and US West with data up to the moderate impact level\. You can use the service for sensitive workloads in the AWS GovCloud \(US\) Region’s authorization boundary with data up to the high impact level\.

You can request access to the AWS FedRAMP Security Packages through the FedRAMP PMO or your AWS Sales Account Manager or, they can be downloaded through **AWS Artifact** at [AWS Artifact](https://aws.amazon.com/artifact/)\.

**Requirements**

To enable FedRAMP support on your ElastiCache for Redis cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running ElastiCache for Redis 3\.2\.6, 4\.0\.10 and later for both cluster mode enabled and disabled to qualify for FedRAMP compliance\.
  + Starting with ElastiCache for Redis versions 3\.2\.6, 4\.0\.10 and later for both cluster mode enabled and disabled, you can also enable additional data security features such as:
    + [ElastiCache for Redis in\-transit encryption \(TLS\)](in-transit-encryption.md)
    + [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
    + [Authenticating users with the Redis AUTH command](auth.md)
+ **Node type requirements** – Your cluster must be running a current\-generation node type — M4, M5, M6g, T2, T3, R4, R5 or R6g\. For more information, see the following: 
  + [Supported node types](CacheNodes.SupportedTypes.md)
  + [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)
+ **FIPS Endpoints requirements** – Your ElastiCache for Redis can be created using the FIPS endpoints available in the following regions:\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-compliance.html)
+ **Security Updates Requirement** – You must regularly update your Redis cluster by the **Recommended Apply by Date**\. You can update the cluster in real\-time and on\-demand to ensure no impact to your business\. For more information, see [Self\-service updates in Amazon ElastiCache](Self-Service-Updates.md)\.

## HIPAA eligibility<a name="elasticache-compliance-hipaa"></a>

The AWS HIPAA Compliance program includes Amazon ElastiCache for Redis as a HIPAA eligible service\.

To use ElastiCache for Redis in compliance with HIPAA, you need to set up a Business Associate Agreement \(BAA\) with AWS\. In addition, your cluster and the nodes within your cluster must satisfy the requirements for engine version, node type, and data security listed following\.

**Requirements**

To enable HIPAA support on your ElastiCache for Redis cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running one of the following ElastiCache for Redis versions to qualify for HIPAA eligibility\.
  + [ElastiCache for Redis version 5\.0\.0 \(enhanced\)](supported-engine-versions.md#redis-version-5-0) or higher\.
  + [ElastiCache for Redis version 4\.0\.10 \(enhanced\)](supported-engine-versions.md#redis-version-4-0-10)
  + [ElastiCache for Redis version 3\.2\.6 \(enhanced\)](supported-engine-versions.md#redis-version-3-2-6)
+ **Node type requirements** – Your cluster must be running a current\-generation node type— M4, M5, M6g, T2, T3, R4, R5 or R6g\. For more information, see the following:
  + [Supported node types](CacheNodes.SupportedTypes.md)
  + [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)
+ **Data security requirements** – Your cluster must enable in\-transit encryption, at\-rest encryption, and Redis AUTH\. For more information, see the following:
  + [ElastiCache for Redis in\-transit encryption \(TLS\)](in-transit-encryption.md)
  + [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
  + [Authenticating users with the Redis AUTH command](auth.md)
+ **Security Updates Requirement ** – You must update your Redis cluster with the latest Service Updates of type **security** by the **Recommended Apply by Date**\. You can update the cluster in real\-time and on\-demand to ensure no impact to your business\. For more information, see [Self\-service updates in Amazon ElastiCache](Self-Service-Updates.md)

By implementing these requirements, ElastiCache for Redis can be used to store, process, and access Protected Health Information \(PHI\) in compliance with HIPAA\. 

For general information about AWS Cloud and HIPAA eligibility, see the following:
+ [HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)
+ [Architecting for HIPAA Security and Compliance on Amazon Web Services](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf)
+ **Security Updates Requirement** – You must regularly update your Redis cluster by the **Recommended Apply by Date**\. You can update the cluster in real\-time and on\-demand to ensure no impact to your business\. For more information, see [Self\-service updates in Amazon ElastiCache](Self-Service-Updates.md)\.

## ElastiCache for Redis PCI DSS compliance<a name="elasticache-compliance-pci"></a>

The AWS PCI DSS Compliance program includes Amazon ElastiCache for Redis as a PCI\-compliant service\. The PCI DSS 3\.2 Compliance Package can be downloaded through [AWS Artifact](https://aws.amazon.com/artifact/)\. For more information, see [AWS PCI DSS Compliance Program](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)\.

**Requirements**

To enable PCI DSS support on your ElastiCache for Redis cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running ElastiCache for Redis 3\.2\.6, 4\.0\.10 and later for both cluster mode enabled and disabled\.
+ **Node type requirements** – Your cluster must be running a current\-generation node type— M4, M5, T2, R4 or R5\. For more information, see the following:
  + [Supported node types](CacheNodes.SupportedTypes.md)
  + [Choosing your node size](nodes-select-size.md#CacheNodes.SelectSize)
+ **Security Updates Requirement** – You must regularly update your Redis cluster by the **Recommended Apply by Date**\. You can update the cluster in real\-time and on\-demand to ensure no impact to your business\. For more information, see [Self\-service updates in Amazon ElastiCache](Self-Service-Updates.md)\.

ElastiCache for Redis also offers Data Security Controls to further secure the cluster to store, process, and transmit sensitive financial data like Customer Cardholder Data \(CHD\) when using the service\.

**Data security options** – For more information, see the following:
+ [ElastiCache for Redis in\-transit encryption \(TLS\)](in-transit-encryption.md)
+ [At\-Rest Encryption in ElastiCache for Redis](at-rest-encryption.md)
+ [Authenticating users with the Redis AUTH command](auth.md)

## Create and seed a new compliant cluster<a name="elasticache-compliance-create-cluster"></a>

To create a compliant cluster, create a new cluster and make sure that your choices fulfill the requirements for the compliance you want\. These requirements can include engine version, node type, encryption, and if needed FIPS endpoints\. If you choose, you can seed a new compliant cluster with data from an existing cluster as you're creating it\. For more information, see the following:
+ [Creating a cluster](Clusters.Create.md)
+ [Creating a Redis replication group from scratch](Replication.CreatingReplGroup.NoExistingCluster.md)
+ [Seeding a new cluster with an externally created backup](backups-seeding-redis.md)

## More information<a name="elasticache-compliance-see-also"></a>

For general information about AWS Cloud compliance, see the following:
+ [Self\-service security updates for compliance](#elasticache-compliance-self-service)
+ [Self\-service updates in Amazon ElastiCache](Self-Service-Updates.md)
+ [AWS Cloud Compliance](https://aws.amazon.com/compliance/)
+ [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
+ [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)
+ [AWS HIPAA Compliance Program](https://aws.amazon.com/compliance/hipaa-compliance/)
+ [Architecting for HIPAA Security and Compliance on Amazon Web Services](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf)
+ [AWS PCI DSS Compliance Program](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)