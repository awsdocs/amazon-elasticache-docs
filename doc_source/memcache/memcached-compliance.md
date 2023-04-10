# Compliance Validation for Amazon ElastiCache<a name="memcached-compliance"></a>

Third\-party auditors assess the security and compliance of AWS services as part of multiple AWS compliance programs, such as SOC, PCI, FedRAMP, and HIPAA\.

To learn whether an AWS service is within the scope of specific compliance programs, see [AWS services in Scope by Compliance Program](http://aws.amazon.com/compliance/services-in-scope/) and choose the compliance program that you are interested in\. For general information, see [AWS Compliance Programs](http://aws.amazon.com/compliance/programs/)\.

You can download third\-party audit reports using AWS Artifact\. For more information, see [Downloading Reports in AWS Artifact](https://docs.aws.amazon.com/artifact/latest/ug/downloading-documents.html)\.

Your compliance responsibility when using AWS services is determined by the sensitivity of your data, your company's compliance objectives, and applicable laws and regulations\. AWS provides the following resources to help with compliance:
+ [Security and Compliance Quick Start Guides](http://aws.amazon.com/quickstart/?awsf.quickstart-homepage-filter=categories%23security-identity-compliance) – These deployment guides discuss architectural considerations and provide steps for deploying baseline environments on AWS that are security and compliance focused\.
+ [Architecting for HIPAA Security and Compliance on Amazon Web Services](https://docs.aws.amazon.com/whitepapers/latest/architecting-hipaa-security-and-compliance-on-aws/welcome.html) – This whitepaper describes how companies can use AWS to create HIPAA\-eligible applications\.
**Note**  
Not all AWS services are HIPAA eligible\. For more information, see the [HIPAA Eligible Services Reference](https://aws.amazon.com/compliance/hipaa-eligible-services-reference/)\.
+ [AWS Compliance Resources](http://aws.amazon.com/compliance/resources/) – This collection of workbooks and guides might apply to your industry and location\.
+ [Evaluating Resources with Rules](https://docs.aws.amazon.com/config/latest/developerguide/evaluate-config.html) in the *AWS Config Developer Guide* – The AWS Config service assesses how well your resource configurations comply with internal practices, industry guidelines, and regulations\.
+ [AWS Security Hub](https://docs.aws.amazon.com/securityhub/latest/userguide/what-is-securityhub.html) – This AWS service provides a comprehensive view of your security state within AWS\. Security Hub uses security controls to evaluate your AWS resources and to check your compliance against security industry standards and best practices\. For a list of supported services and controls, see [Security Hub controls reference](https://docs.aws.amazon.com/securityhub/latest/userguide/securityhub-controls-reference.html)\.
+ [AWS Audit Manager](https://docs.aws.amazon.com/audit-manager/latest/userguide/what-is.html) – This AWS service helps you continuously audit your AWS usage to simplify how you manage risk and compliance with regulations and industry standards\.

## Self\-Service Security Updates for Compliance<a name="elasticache-compliance-self-service-memcached"></a>

ElastiCache offers a self\-service software update feature called **Service Updates** via the Console, API and CLI\. Using this feature, you can manage security updates on your clusters on\-demand and in real\-time\. This feature allows you to control when you update clusters with the latest required security fixes, minimizing the impact on your business\.

Security updates are released via the **Service Updates** feature\. They are specified by the **Update Type** field of value **security update**\. The Service Update has corresponding **Severity** and **Recommended Apply by Date** fields\. In order to maintain compliance of your clusters, you must apply the available updates by the **Recommended Apply by Date**\. The field **SLA Met** reflects your cluster’s compliance status\. 

**Note**  
If you do not apply the Service Update by the recommended date or when the Service Update expires, ElastiCache will not take any action to apply the update on your behalf\.  
You will be notified of the Service Updates applicable to your Redis clusters via an announcement on the Redis console, email, Amazon SNS, CloudWatch events and AWS Health Dashboard\. For more information on Self\-Service Maintenance see [Service updates in ElastiCache for Memcached](Self-Service-Updates.md)\.   
   
CloudWatch events and AWS Health Dashboard are not supported in the following regions:  
us\-gov\-west\-1 
us\-gov\-east\-1
cn\-north\-1
cn\-northwest\-1

## HIPAA Eligibility<a name="elasticache-compliance-hipaa-memcached"></a>

The AWS HIPAA Compliance program includes Amazon ElastiCache for Memcached as a HIPAA eligible service\.

To use ElastiCache for Memcached in compliance with HIPAA, you need to set up a Business Associate Agreement \(BAA\) with AWS\. In addition, your cluster and the nodes within your cluster must satisfy the requirements for engine version, node type, and data security listed following\.

**Requirements**

To enable HIPAA support on your ElastiCache for Memcached cluster, your cluster and nodes within the cluster must satisfy the following requirements\.
+ **Engine version requirements** – Your cluster must be running engine version 1\.16\.12 to qualify for HIPAA eligibility\.
+ **Node type requirements** – Your cluster must be running a current\-generation node type\. For more information, see the following:
  + [Supported node types](CacheNodes.SupportedTypes.md)
  + [Choosing your Memcached node size](nodes-select-size.md#CacheNodes.SelectSize)
+ **Data security requirements** – Your cluster must enable in\-transit encryption\. For more information, see [ElastiCache in\-transit encryption \(TLS\)](in-transit-encryption-mc.md),
+ **Security Updates Requirement ** – You must update your Memcached cluster with the latest Service Updates of type **security** by the **Recommended Apply by Date**\. You can update the cluster in real\-time and on\-demand to ensure no impact to your business\. For more information, see [Service updates in ElastiCache for Memcached](Self-Service-Updates.md)

By implementing these requirements, ElastiCache for Memcached can be used to store, process, and access Protected Health Information \(PHI\) in compliance with HIPAA\. 

For general information about AWS Cloud and HIPAA eligibility, see the following:
+ [HIPAA Compliance](https://aws.amazon.com/compliance/hipaa-compliance/)
+ [Architecting for HIPAA Security and Compliance on Amazon Web Services](https://d0.awsstatic.com/whitepapers/compliance/AWS_HIPAA_Compliance_Whitepaper.pdf)
+ **Security Updates Requirement** – You must regularly update your Redis cluster by the **Recommended Apply by Date**\. You can update the cluster in real\-time and on\-demand to ensure no impact to your business\. For more information, see [Service updates in ElastiCache for Memcached](Self-Service-Updates.md)\.