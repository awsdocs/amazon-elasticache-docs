# Managing Costs with Reserved Nodes<a name="reserved-nodes"></a>

Reserving one or more nodes may be a way for you to reduce costs\. Reserved nodes are charged an up front fee that depends upon the node type and the length of reservation – 1 or 3 years\. In addition to the up front charge there is an hourly usage charge which is significantly less than the hourly usage charge you incur with On\-Demand nodes\. To determine whether reserved nodes would be a cost savings for your use cases, determine the node size and number of nodes you need, estimate the usage of the node, then compare the total cost to you using On\-Demand nodes versus reserved nodes\. You can mix and match reserved and On\-Demand node usage in your clusters\. For pricing information, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.

You can use the AWS Management Console, the AWS CLI, or the ElastiCache API to list and purchase available reserved node offerings\.

For more information on reserved nodes, see [Amazon ElastiCache Reserved Cache Nodes](https://aws.amazon.com/elasticache/reserved-cache-nodes/)\.

**Topics**
+ [Standard Reserved Node Cache Offerings](#reserved-nodes-standard)
+ [Legacy Reserved Node Cache Offerings](#reserved-nodes-utilization)
+ [Getting Info About Reserved Node Offerings](reserved-nodes-offerings.md)
+ [Purchasing a Reserved Node](reserved-nodes-purchasing.md)
+ [Getting Info About Your Reserved Nodes](reserved-nodes-describing.md)

## Standard Reserved Node Cache Offerings<a name="reserved-nodes-standard"></a>

When you purchase a standard reserved node instance \(RI\) in Amazon ElastiCache, you purchase a commitment to getting a discounted rate on a specific cache node instance type and region for the duration of the reserved node instance\. To use an Amazon ElastiCache reserved node instance, you create a new ElastiCache node instance, just as you would for an on\-demand instance\.

 The new node instance that you create must exactly match the specifications of the reserved node instance\. If the specifications of the new node instance match an existing reserved node instance for your account, you are billed at the discounted rate offered for the reserved node instance\. Otherwise, the node instance is billed at an on\-demand rate\. These standard RIs are available from R5 and M5 instance families onwards\. 

**Note**  
All three offering types discussed next are available in one\-year and three\-year terms\.

**Offering Types**  
**No Upfront ** RI provides access to a reserved ElastiCache instance without requiring an upfront payment\. Your *No Upfront* reserved ElastiCache instance bills a discounted hourly rate for every hour within the term, regardless of usage\. 

**Partial Upfront** RI requires a part of the reserved ElasticCache instance to be paid upfront\. The remaining hours in the term are billed at a discounted hourly rate, regardless of usage\. This option is the replacement for the legacy *Heavy Utilization* option, which is explained in the next section\.

**All Upfront** RI requires full payment to be made at the start of the RI term, with no other costs incurred for the remainder of the term, regardless of the number of hours used\. 

## Legacy Reserved Node Cache Offerings<a name="reserved-nodes-utilization"></a>

There are three levels of legacy node reservations – Heavy Utilization, Medium Utilization, and Light Utilization\. Nodes can be reserved at any utilization level for either 1 or 3 years\. The node type, utilization level, and reservation term will impact your total costs\. You should verify the savings that reserved nodes can provide your business by comparing various models before you purchase reserved nodes\.

Nodes purchased at one utilization level or term cannot be converted to a different utilization level or term\.

**Utilization Levels**  
*Heavy Utilization reserved nodes* enable workloads that have a consistent baseline of capacity or run steady\-state workloads\. Heavy Utilization reserved nodes require a high up\-front commitment, but if you plan to run more than 79 percent of the reserved node term you can earn the largest savings \(up to 70 percent off of the On\-Demand price\)\. With Heavy Utilization reserved nodes you pay a one\-time fee, followed by a lower hourly fee for the duration of the term regardless of whether or not your node is running\.

*Medium Utilization reserved nodes* are the best option if you plan to leverage your reserved nodes a substantial amount of the time, but you want either a lower one\-time fee or the flexibility to stop paying for your node when you shut it off\. Medium Utilization reserved nodes are a more cost\-effective option when you plan to run more than 40 percent of the reserved nodes term\. This option can save you up to 64 percent off of the On\-Demand price\. With Medium Utilization reserved nodes, you pay a slightly higher one\-time fee than with Light Utilization reserved nodes, and you receive lower hourly usage rates when you run a node\.

*Light Utilization reserved nodes* are ideal for periodic workloads that run only a couple of hours a day or a few days per week\. Using Light Utilization reserved nodes, you pay a one\-time fee followed by a discounted hourly usage fee when your node is running\. You can start saving when your node is running more than 17 percent of the reserved node term, and you can save up to 56 percent off of the On\-Demand rates over the entire term of your reserved node\.


**Legacy Reserved Cache Node Offerings**  

| Offering | Up\-Front Cost | Usage Fee | Advantage | 
| --- | --- | --- | --- | 
|  Heavy Utilization |  Highest |  Lowest hourly fee\. Applied to the whole term whether or not you're using the reserved node\. |  Lowest overall cost if you plan to run your reserved nodes more than 79 percent of a three\-year term\. | 
|  Medium Utilization |  Medium |  Hourly usage fee charged for each hour the node is running\. No hourly charge when the node is not running\. |  Suitable for elastic workloads or when you expect moderate usage, more than 40 percent of a three\-year term\. | 
|  Light Utilization |  Lowest |  Hourly usage fee charged for each hour the node is running\. No hourly charge when the node is not running\. Highest hourly fees of all the offering types, but fees apply only when the reserved node is running\. |  Highest overall cost if you plan to run all of the time; however, lowest overall cost if you anticipate you will use your reserved node infrequently, more than about 15 percent of a three\-year term\. | 
|  On\-Demand Use \(No reserved nodes\) |  None |  Highest hourly fee\. Applied whenever the node is running\. |  Highest hourly cost\. | 

For more information, see [Amazon ElastiCache Pricing](https://aws.amazon.com/elasticache/pricing/)\.