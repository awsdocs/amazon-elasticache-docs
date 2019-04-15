# ElastiCache for Redis Compliance<a name="elasticache-compliance"></a>

In this section, you can find the compliance requirements and controls offered when using Amazon ElastiCache for Redis\. 

**Topics**
+ [ElastiCache for Redis FedRAMP Compliance](#elasticache-compliance-fedramp)
+ [HIPAA Compliance in ElastiCache for Redis](#elasticache-compliance-hipaa)
+ [ElastiCache for Redis PCI DSS Compliance](#elasticache-compliance-pci)
+ [Create and Seed a New Compliant Cluster](#elasticache-compliance-create-cluster)
+ [More Information](#elasticache-compliance-see-also)

## ElastiCache for Redis FedRAMP Compliance<a name="elasticache-compliance-fedramp"></a>

The AWS FedRAMP Compliance program includes Amazon ElastiCache for Redis as a FedRAMP\-authorized service\. If you are a federal or commercial customer, you can use the service to process and store sensitive workloads in AWS US East and US West with data up to the moderate impact level\. You can use the service for sensitive workloads in the AWS GovCloud \(US\) Region’s authorization boundary with data up to the high impact level\.

You can request access to the AWS FedRAMP Security Packages through the FedRAMP PMO or your AWS Sales Account Manager or, they can be downloaded through **AWS Artifact** at [https://aws\.amazon\.com/artifact/](https://aws.amazon.com/artifact/)\.

**Requirements**

To enable FedRAMP support on your ElastiCache for Redis cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running one of the following ElastiCache for Redis versions to qualify for FedRAMP compliance\.
  + [ElastiCache for Redis Version 5\.0\.0 \(Enhanced\)](supported-engine-versions.md#redis-version-5-0) or later\.

    Starting with ElastiCache for Redis version 4\.0\.10 you can also enable additional data security features such as:
    + [ElastiCache for Redis In\-Transit Encryption \(TLS\)](in-transit-encryption.md)
    + [ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)
    + [Authenticating Users with Redis AUTH](auth.md)
+ **Node type requirements** – Your cluster must be running a current\-generation node type—M4, M5, T2, R4 or R5\. For more information, see the following: 
  + [Supported Node Types](CacheNodes.SupportedTypes.md)
  + [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)
+ **FIPS Endpoints requirements** – Your ElastiCache for Redis can be created using the FIPS endpoints available in the following regions:\.    
[\[See the AWS documentation website for more details\]](http://docs.aws.amazon.com/AmazonElastiCache/latest/red-ug/elasticache-compliance.html)

## HIPAA Compliance in ElastiCache for Redis<a name="elasticache-compliance-hipaa"></a>

The AWS HIPAA Compliance program includes Amazon ElastiCache for Redis as a HIPAA Eligible Service\.

To use ElastiCache for Redis in compliance with HIPAA, you need to execute a Business Associate Agreement \(BAA\) with AWS\. In addition, your cluster and the nodes within your cluster must satisfy the requirements for engine version, node type, and data security listed following\.

**Requirements**

To enable HIPAA support on your ElastiCache for Redis cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running one of the following ElastiCache for Redis versions to qualify for HIPAA compliance\.
  + [ElastiCache for Redis Version 5\.0\.0 \(Enhanced\)](supported-engine-versions.md#redis-version-5-0)
  + [ElastiCache for Redis Version 4\.0\.10 \(Enhanced\)](supported-engine-versions.md#redis-version-4-0-10)
  + [ElastiCache for Redis Version 3\.2\.6 \(Enhanced\)](supported-engine-versions.md#redis-version-3-2-6)
+ **Node type requirements** – Your cluster must be running a current\-generation node type—M3, M4, M5, T2, R3, R4 or R5\. For more information, see the following:
  + [Supported Node Types](CacheNodes.SupportedTypes.md)
  + [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)
+ **Data security requirements** – Your cluster must enable in\-transit encryption, at\-rest encryption, and Redis AUTH\. For more information, see the following:
  + [ElastiCache for Redis In\-Transit Encryption \(TLS\)](in-transit-encryption.md)
  + [ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)
  + [Authenticating Users with Redis AUTH](auth.md)

By implementing these requirements, ElastiCache for Redis can be used to store, process, and access Protected Health Information \(PHI\) in compliance with HIPAA\. 

For general information about AWS Cloud and HIPAA compliance, see the following:
+ [HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)
+ [Architecting for HIPAA Security and Compliance on Amazon Web Services](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf)

## ElastiCache for Redis PCI DSS Compliance<a name="elasticache-compliance-pci"></a>

The AWS PCI DSS Compliance program includes Amazon ElastiCache for Redis as a PCI\-compliant service\. The PCI DSS 3\.2 Compliance Package can be downloaded through **AWS Artifact** at [https://aws\.amazon\.com/artifact/](https://aws.amazon.com/artifact/)\. For more information, see [AWS PCI DSS Compliance Program](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)\.

**Requirements**

To enable PCI DSS support on your ElastiCache for Redis cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running one of the following ElastiCache for Redis versions to qualify for PCI DSS compliance\.
  + [ElastiCache for Redis Version 5\.0\.0 \(Enhanced\)](supported-engine-versions.md#redis-version-5-0) or later\.
+ **Node type requirements** – Your cluster must be running a current\-generation node type— M4, M5, T2, R4 or R5\. For more information, see the following:
  + [Supported Node Types](CacheNodes.SupportedTypes.md)
  + [Choosing Your Node Size](nodes-select-size.md#CacheNodes.SelectSize)

ElastiCache for Redis also offers Data Security Controls to further secure the cluster to store, process, and transmit sensitive financial data like Customer Cardholder Data \(CHD\) when using the service\.

**Data security options** – For more information, see the following:
+ [ElastiCache for Redis In\-Transit Encryption \(TLS\)](in-transit-encryption.md)
+ [ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)
+ [Authenticating Users with Redis AUTH](auth.md)

## Create and Seed a New Compliant Cluster<a name="elasticache-compliance-create-cluster"></a>

To create a compliant cluster, create a new cluster and make sure that your choices fulfill the requirements for the compliance you want\. These requirements can include engine version, node type, encryption, and if needed FIPS endpoints\. If you choose, you can seed a new compliant cluster with data from an existing cluster as you're creating it\. For more information, see the following:
+ [Creating a Cluster](Clusters.Create.md)
+ [Creating a Redis Replication Group from Scratch](Replication.CreatingReplGroup.NoExistingCluster.md)
+ [Seeding a New Cluster with an Externally Created Backup](backups-seeding-redis.md)

## More Information<a name="elasticache-compliance-see-also"></a>

For general information about AWS Cloud compliance, see the following:
+ [AWS Cloud Compliance](https://aws.amazon.com/compliance/)
+ [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)
+ [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)
+ [AWS HIPAA Compliance Program](https://aws.amazon.com/compliance/hipaa-compliance/)
+ [Architecting for HIPAA Security and Compliance on Amazon Web Services](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf)
+ [AWS PCI DSS Compliance Program](https://aws.amazon.com/compliance/pci-dss-level-1-faqs/)