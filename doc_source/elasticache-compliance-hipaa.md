# HIPAA Compliance for Amazon ElastiCache for Redis<a name="elasticache-compliance-hipaa"></a>

You can find information about Amazon ElastiCache for Redis requirements for HIPAA compliance at [ElastiCache for Redis HIPAA Requirements](#elasticache-compliance-hipaa-requirements)\. There is no additional charge beyond the normal ElastiCache for Redis charges for making your cluster HIPAA\-compliant\.

The AWS HIPAA compliance program includes Amazon ElastiCache for Redis as a HIPAA Eligible Service\. If you have an executed [Business Associate Agreement](https://aws.amazon.com/compliance/hipaa-compliance/) \(BAA\) with AWS, you can use Amazon ElastiCache for Redis to build your HIPAA\-compliant applications containing protected health information \(PHI\)\. For more information, see [HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)\. AWS Services in Scope have been fully assessed by a third\-party auditor and result in a certification, attestation of compliance, or Authority to Operate \(ATO\)\. For more information, see [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)\.

## ElastiCache for Redis HIPAA Requirements<a name="elasticache-compliance-hipaa-requirements"></a>

To enable HIPAA support on your ElastiCache for Redis cluster, in addition to executing the BAA, your cluster and nodes within the cluster must satisfy the following requirements:

+ **Data security requirements** – Your cluster must enable in\-transit encryption, at\-rest encryption, and Redis AUTH\. For more information, see the following:

  + [Amazon ElastiCache for Redis In\-Transit Encryption](in-transit-encryption.md)

  + [Amazon ElastiCache for Redis At\-Rest Encryption](at-rest-encryption.md)

  + [Authenticating Users with AUTH \(Redis\)](auth.md)

+ **Engine version requirements** – Your cluster must be running ElastiCache for Redis version 3\.2\.6 to qualify for HIPAA compliance\. For more information, see [ElastiCache for Redis Version 3\.2\.6 \(Enhanced\)](SelectEngine.RedisVersions.md#SelectEngine.RedisVersions.3-2-6)\.

+ **Node type requirements** – Your cluster must be running a current\-generation node type—M3, M4, T2, or R3\. For more information, see the following:

  + [Supported Node Types](CacheNodes.SupportedTypes.md)

  + [Choosing Your Node Size for Redis Clusters](CacheNodes.SelectSize.md#CacheNodes.SelectSize.Redis)

When you have created a compliant ElastiCache for Redis cluster, you can start storing PHI\. If you want, you can seed the new cluster with data from an existing cluster\. For more information, see the following:

+ [Creating a Cluster](Clusters.Create.md)

+ [Creating a Redis Cluster with Replicas from Scratch](Replication.CreatingReplGroup.NoExistingCluster.md)

+ [Seeding a New Cluster with an Externally Created Backup \(Redis\)](backups-seeding-redis.md)

For general information about AWS Cloud and HIPAA compliance, see the following:

+ [AWS Cloud Compliance](https://aws.amazon.com/compliance/)

+ [Shared Responsibility Model](https://aws.amazon.com/compliance/shared-responsibility-model/)

+ [HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)

+ [AWS Services in Scope by Compliance Program](https://aws.amazon.com/compliance/services-in-scope/)