# Managing maintenance<a name="maintenance-window"></a>

Every cluster has a weekly maintenance window during which any system changes are applied\. If you don't specify a preferred maintenance window when you create or modify a cluster, ElastiCache assigns a 60\-minute maintenance window within your region's maintenance window on a randomly chosen day of the week\.

The 60\-minute maintenance window is chosen at random from an 8\-hour block of time per region\. The following table lists the time blocks for each region from which the default maintenance windows are assigned\. You may choose a preferred maintenance window outside the region's maintenance window block\.


| Region Code | Region Name | Region Maintenance Window | 
| --- | --- | --- | 
| ap\-northeast\-1 | Asia Pacific \(Tokyo\) Region | 13:00–21:00 UTC | 
| ap\-northeast\-2 | Asia Pacific \(Seoul\) Region | 12:00–20:00 UTC | 
| ap\-northeast\-3 | Asia Pacific \(Osaka\) Region | 12:00–20:00 UTC | 
| ap\-south\-1 | Asia Pacific \(Mumbai\) Region | 17:30–1:30 UTC | 
| ap\-southeast\-1 | Asia Pacific \(Singapore\) Region | 14:00–22:00 UTC | 
| cn\-north\-1 | China \(Beijing\) Region | 14:00–22:00 UTC | 
| cn\-northwest\-1 | China \(Ningxia\) Region | 14:00–22:00 UTC | 
| ap\-east\-1 | Asia Pacific \(Hong Kong\) Region | 13:00–21:00 UTC | 
| ap\-southeast\-2 | Asia Pacific \(Sydney\) Region | 12:00–20:00 UTC | 
| eu\-west\-3 | EU \(Paris\) Region | 23:59–07:29 UTC | 
| af\-south\-1 | Africa \(Cape Town\) Region | 13:00–21:00 UTC | 
| eu\-central\-1 | Europe \(Frankfurt\) Region | 23:00–07:00 UTC | 
| eu\-west\-1 | Europe \(Ireland\) Region | 22:00–06:00 UTC | 
| eu\-west\-2 | Europe \(London\) Region | 23:00–07:00 UTC | 
| me\-south\-1 | Middle East \(Bahrain\) Region | 13:00–21:00 UTC | 
| eu\-south\-1 | Europe \(Milan\) Region | 21:00–05:00 UTC | 
| sa\-east\-1 | South America \(São Paulo\) Region | 01:00–09:00 UTC | 
| us\-east\-1 | US East \(N\. Virginia\) Region | 03:00–11:00 UTC | 
| us\-east\-2 | US East \(Ohio\) Region | 04:00–12:00 UTC | 
| us\-gov\-west\-1 | AWS GovCloud \(US\) region | 06:00–14:00 UTC | 
| us\-west\-1 | US West \(N\. California\) Region | 06:00–14:00 UTC | 
| us\-west\-2 | US West \(Oregon\) Region | 06:00–14:00 UTC | 

**Changing your Cluster's Maintenance Window**  
The maintenance window should fall at the time of lowest usage and thus might need modification from time to time\. You can modify your cluster to specify a time range of up to 24 hours in duration during which any maintenance activities you have requested should occur\. Any deferred or pending cluster modifications you requested occur during this time\.

**More information**  
For information on your maintenance window and node replacement, see the following:
+ [ElastiCache Maintenance](https://aws.amazon.com/elasticache/elasticache-maintenance/)—FAQ on maintenance and node replacement
+ [Replacing nodes](CacheNodes.NodeReplacement.md)—Managing node replacement
+ [Modifying an ElastiCache cluster](Clusters.Modify.md)—Changing a cluster's maintenance window