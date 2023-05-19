# Enabling in\-transit encryption on a Redis Cluster using Python<a name="in-transit-encryption-enable-python"></a>

The following guide will demonstrate how to enable in\-transit encryption on a Redis 7\.0 cluster that was originally created with in\-transit encryption disabled\. TCP and TLS clients will continue communicating with the cluster during this process without downtime\.

Boto3 will get the credentials it needs \(`aws_access_key_id`, `aws_secret_access_key`, and `aws_session_token`\) from the environment variables\. Those credentials will be pasted in advance in the same bash terminal where we will run `python3` to process the Python code shown in this guide\. The code in the example below was process from an EC2 instance that was launched in the same VPC that will be used to create the ElastiCache Redis Cluster in it\.

**Note**  
The following examples use the boto3 SDK for ElastiCache management operations \(cluster or user creation\) and redis\-py/redis\-py\-cluster for data handling\.
You must use at least boto3 version \(=\~\) 1\.26\.39 to use the online TLS migration with the cluster modification API\.
ElastiCache supports online TLS migration only for Redis Clusters with version 7\.0 or above\. So if you have a cluster running Redis version earlier than 7\.0, you’ll need to upgrade the Redis version of your cluster\. For more information on version differences, see [Major version behavior and compatibility differences](VersionManagementConsiderations.md)\.

**Topics**
+ [Define the string constants that will launch the ElastiCache Redis Cluster](#enable-python-define-constants)
+ [Define the classes for the cluster configuration](#enable-python-define-classes)
+ [Define a class that will represent the cluster itself](#enable-python-define-classes-cluster)
+ [\(Optional\) Create a wrapper class to demo client connection to Redis cluster](#enable-python-create-wrapper)
+ [Create the main function that demos the process of changing in\-transit encryption configuration](#enable-python-main-function)
+ [Best practices to establish connections before, during, and after enabling in\-transit encryption](#enable-python-best-practices)

## Define the string constants that will launch the ElastiCache Redis Cluster<a name="enable-python-define-constants"></a>

First, let’s define some simple Python string constants that will hold the names of the AWS entities required to create the ElastiCache cluster such as `security-group`, `Cache Subnet group`, and a `default parameter group`\. All of these AWS entities must be created in advance in your AWS account in the Region you are willing to use\.

```
#Constants definitions 
SECURITY_GROUP = "sg-0492aa0a29c558427"
CLUSTER_DESCRIPTION = "This cluster has been launched as part of the online TLS migration user guide"
EC_SUBNET_GROUP = "client-testing"
DEFAULT_PARAMETER_GROUP_REDIS_7_CLUSTER_MODE_ENABLED = "default.redis7.cluster.on"
```

## Define the classes for the cluster configuration<a name="enable-python-define-classes"></a>

Now, let’s define some simple Python classes that will represent a configuration of a cluster, which will hold metadata about the cluster such as the Redis version, the instance type, and whether in\-transit encryption \(TLS\) is enabled or disabled\.

```
#Class definitions

class Config:
    def __init__(
        self,
        instance_type: str = "cache.t4g.small",
        version: str = "7.0",
        multi_az: bool = True,
        TLS: bool = True,
        name: str = None,
    ):
        self.instance_type = instance_type
        self.version = version
        self.multi_az = multi_az
        self.TLS = TLS
        self.name = name or f"alonare-test"

    def create_base_launch_request(self):
        return {
            "ReplicationGroupId": self.name,
            "TransitEncryptionEnabled": self.TLS,
            "MultiAZEnabled": self.multi_az,
            "CacheNodeType": self.instance_type,
            "Engine": "redis",
            "EngineVersion": self.version,
            "CacheSubnetGroupName": EC_SUBNET_GROUP ,
            "CacheParameterGroupName": DEFAULT_PARAMETER_GROUP_REDIS_7_CLUSTER_MODE_ENABLED ,
            "ReplicationGroupDescription": CLUSTER_DESCRIPTION,
            "SecurityGroupIds": [SECURITY_GROUP],
        }
        
class ConfigCME(Config):
    def __init__(
        self,
        instance_type: str = "cache.t4g.small",
        version: str = "7.0",
        multi_az: bool = True,
        TLS: bool = True,
        name: str = None,
        num_shards: int = 2,
        num_replicas_per_shard: int = 1,
    ):
        super().__init__(instance_type, version, multi_az, TLS, name)
        self.num_shards = num_shards
        self.num_replicas_per_shard = num_replicas_per_shard

    def create_launch_request(self) -> dict:
        launch_request = self.create_base_launch_request()
        launch_request["NumNodeGroups"] = self.num_shards
        launch_request["ReplicasPerNodeGroup"] = self.num_replicas_per_shard
        return launch_request
```

## Define a class that will represent the cluster itself<a name="enable-python-define-classes-cluster"></a>

Now, let’s define some simple Python classes that will represent the ElastiCache Redis Cluster itself\. This class will have a client field which will hold a boto3 client for ElastiCache management operations such as creating the cluster and querying the ElastiCache API\.

```
import botocore.config
import boto3

# Create boto3 client
def init_client(region: str = "us-east-1"):
    config = botocore.config.Config(retries={"max_attempts": 10, "mode": "standard"})
    init_request = dict()
    init_request["config"] = config
    init_request["service_name"] = "elasticache"
    init_request["region_name"] = region
    return boto3.client(**init_request) 
 
 
class ElastiCacheClusterBase:
    def __init__(self, name: str):
        self.name = name
        self.elasticache_client = init_client()

    def get_first_replication_group(self):
        return self.elasticache_client.describe_replication_groups(
        ReplicationGroupId=self.name
        )["ReplicationGroups"][0]
 
    def get_status(self) -> str:
        return self.get_first_replication_group()["Status"]
 
    def get_transit_encryption_enabled(self) -> bool:
        return self.get_first_replication_group()["TransitEncryptionEnabled"]
 
    def is_available(self) -> bool:
        return self.get_status() == "available"
        
    def is_modifying(self) -> bool:
        return self.get_status() == "modifying"
        
    def wait_for_available(self):
        while True:
            if self.is_available():
                break
            else:
                time.sleep(5)

    def wait_for_modifying(self):
        while True:
            if self.is_modifying():
                break
            else:
                time.sleep(5)
                
    def delete_cluster(self) -> bool:
        self.elasticache_client.delete_replication_group(
            ReplicationGroupId=self.name, RetainPrimaryCluster=False
        )
        
    def modify_transit_encryption_mode(self, new_transit_encryption_mode: str):
        # generate api call to migrate the cluster to TLS preffered or to TLS required
            self.elasticache_client.modify_replication_group(
                ReplicationGroupId=self.name,
                TransitEncryptionMode=new_transit_encryption_mode,
                TransitEncryptionEnabled=True,
                ApplyImmediately=True,
            )  
        self.wait_for_modifying()
              
 class ElastiCacheClusterCME(ElastiCacheClusterBase):
    def __init__(self, name: str):
        super().__init__(name)

    @classmethod
    def launch(cls, config: ConfigCME = None) -> ElastiCacheClusterCME:
        config = config or ConfigCME()
        print(config)
        new_cluster = ElastiCacheClusterCME(config.name)
        launch_request = config.create_launch_request()
        new_cluster.elasticache_client.create_replication_group(**launch_request)
        new_cluster.wait_for_available()
        return new_cluster

    def get_configutration_endpoint(self) -> str:
        return self.get_first_replication_group()["ConfigurationEndpoint"]["Address"]
     
#Since the code can throw exceptions, we define this class to make the code more readable and 
#so we won't forget to delete the cluster    
class ElastiCacheCMEManager:
    def __init__(self, config: ConfigCME = None):
        self.config = config or ConfigCME()

    def __enter__(self) -> ElastiCacheClusterCME:
        self.cluster = ElastiCacheClusterCME.launch(self.config)
        return self.cluster 
          
    def __exit__(self, exc_type, exc_val, exc_tb):
        self.cluster.delete_cluster()
```

## \(Optional\) Create a wrapper class to demo client connection to Redis cluster<a name="enable-python-create-wrapper"></a>

Now, let’s create a wrapper class for the `redis-py-cluster` client\. This wrapper class will support pre\-filling the cluster with some keys and then doing random repeated `get` commands\.

**Note**  
This is an optional step but it simplifies the code of the main function that comes in a later step\.

```
import redis
improt random
from time import perf_counter_ns, time


class DowntimeTestClient:
    def __init__(self, client):
        self.client = client

        # num of keys prefilled
        self.prefilled = 0
        # percent of get above prefilled
        self.percent_get_above_prefilled = 10 # nil result expected when get hit above prefilled
        # total downtime in nano seconds 
        self.downtime_ns = 0
        # num of success and fail operations
        self.success_ops = 0
        self.fail_ops = 0
        self.connection_errors = 0
        self.timeout_errors = 0
        

    def replace_client(self, client):
        self.client = client

    def prefill_data(self, timelimit_sec=60):
        end_time = time() + timelimit_sec
        while time() < end_time:
            self.client.set(self.prefilled, self.prefilled)
            self.prefilled += 1

    # unsuccesful operations throw exceptions
    def _exec(self, func):
        try:
            start_ns = perf_counter_ns()
            func()
            self.success_ops += 1
            elapsed_ms = (perf_counter_ns() - start_ns) // 10 ** 6
            # upon succesful execution of func
            # reset random_key to None so that the next command
            # will use a new random key
            self.random_key = None

        except Exception as e:
            elapsed_ns = perf_counter_ns() - start_ns
            self.downtime_ns += elapsed_ns
            # in case of failure- increment the relevant counters so that we will keep track 
            # of how many connection issues we had while trying to communicate with
            # the cluster.
            self.fail_ops += 1
            if e.__class__ is redis.exceptions.ConnectionError:
                self.connection_errors += 1
            if e.__class__ is redis.exceptions.TimeoutError:
                self.timeout_errors += 1

    def _repeat_exec(self, func, seconds):
        end_time = time() + seconds
        while time() < end_time:
            self._exec(func)

    def _new_random_key_if_needed(self, percent_above_prefilled):
        if self.random_key is None:
            max = int((self.prefilled * (100 + percent_above_prefilled)) / 100)
            return random.randint(0, max)
        return self.random_key

    def _random_get(self):
        key = self._new_random_key_if_needed(self.percent_get_above_prefilled)
        result = self.client.get(key)
        # we know the key was set for sure only in the case key < self.prefilled
        if key < self.prefilled:
            assert result.decode("UTF-8") == str(key)


    def repeat_get(self, seconds=60):
        self._repeat_exec(self._random_get, seconds)

    def get_downtime_ms(self) -> int:
        return self.downtime_ns // 10 ** 6


    def do_get_until(self, cond_check):
        while not cond_check():
            self.repeat_get()
        # do one more get cycle once condition is met
        self.repeat_get()
```

## Create the main function that demos the process of changing in\-transit encryption configuration<a name="enable-python-main-function"></a>

Now, let’s define the main function, which will do the following:

1. Create the cluster using boto3 ElastiCache client\.

1. Initialize the `redis-py-cluster` client that will connect to the cluster with a clear TCP connection without TLS\.

1. The `redis-py-cluster` client prefills the cluster with some data\. 

1. The boto3 client will trigger TLS migration from no\-TLS to TLS preferred\.

1. While the cluster is being migrated to TLS `Preferred`, the `redis-py-cluster` TCP client will send repeated `get` operations to the cluster until the migration is finished\.

1. After the migration to TLS `Preferred` is finished, we will assert that the cluster supports in\-transit encryption\. Afterwards, we will create a `redis-py-cluster` client that will connect to the cluster with TLS\.

1. We will send some `get` commands using the new TLS client and the old TCP client\.

1. The boto3 client will trigger TLS migration from TLS `Preferred` to TLS required\.

1. While the cluster is being migrated to TLS required, the redis\-py\-cluster TLS client will send repeated `get` operations to the cluster until the migration is finished\.

```
import redis

def init_cluster_client(
    cluster: ElastiCacheClusterCME, prefill_data: bool, TLS: bool = True) -> DowntimeTestClient:
    # we must use for the host name the cluster configuration endpoint. 
    redis_client = redis.RedisCluster(
        host=cluster.get_configutration_endpoint(), ssl=TLS, socket_timeout=0.25, socket_connect_timeout=0.1
    )
    test_client = DowntimeTestClient(redis_client)
    if prefill_data:
        test_client.prefill_data()
    return test_client

if __name__ == '__main__':
    config = ConfigCME(TLS=False, instance_type="cache.m5.large")

    with ElastiCacheCMEManager(config) as cluster:
        # create a client that will connect to the cluster with clear tcp connection
        test_client_tcp = init_cluster_client(cluster, prefill_data=True, TLS=False)
        
       # migrate the cluster to TLS Preferred
        cluster.modify_transit_encryption_mode(new_transit_encryption_mode="preferred")
        
        # do repeated get commands until the cluster finishes the migration to TLS Preferred
        test_client_tcp.do_get_until(cluster.is_available)
        
       # verify that in transit encryption is enabled so that clients will be able to connect to the cluster with TLS
        assert cluster.get_transit_encryption_enabled() == True
        
       # create a client that will connect to the cluster with TLS connection. 
        # we must first make sure that the cluster indeed supports TLS
        test_client_tls = init_cluster_client(cluster, prefill_data=True, TLS=True)
        
        # by doing get commands with the tcp client for 60 more seconds
       # we can verify that the existing tcp connection to the cluster still works 
        test_client_tcp.repeat_get(seconds=60)
        
        # do get commands with the new TLS client for 60 more seconds
        test_client_tcp.repeat_get(seconds=60)
        
       # migrate the cluster to TLS required
        cluster.modify_transit_encryption_mode(new_transit_encryption_mode="required")
        
       # from this point the tcp clients will be disconnected and we must not use them anymore.
       # do get commands with the TLS client until the cluster finishes migartion to TLS required mode.
        test_client_tls.do_get_until(cluster.is_available)
```

## Best practices to establish connections before, during, and after enabling in\-transit encryption<a name="enable-python-best-practices"></a>

### Before enabling in\-transit encryption: make sure you have proper DNS records handling<a name="enable-python-best-practices-before"></a>

**Note**  
We are changing and deleting old endpoints during this process\. Incorrect usage of the endpoints can result in the Redis client using old and deleted endpoints that will prevent it from connecting to the cluster\.

While the cluster is being migrated from no\-TLS to TLS\-preferred, the old per\-node DNS records are kept and the new per\-node DNS records are being generated in a different format\. TLS\-enabled clusters use a different format of DNS records than non\-TLS\-enabled clusters\. ElastiCache will keep both DNS records when a cluster is configured in encryption mode: Preferred so that Applications and other Redis Clients can switch between them\. The following changes in the DNS records take place during the TLS\-migration process:

#### Description of the changes in the DNS records that take place when enabling in\-transit encryption<a name="enable-python-best-practices-before-desc"></a>

**For CME clusters**

When a cluster is set to ‘transit encryption mode: preferred’:
+ The original cluster endpoints for non\-TLS enabled cluster will remain active\. There will be no downtime when cluster is re\-configured form TLS encryption mode ‘none’ to ‘preferred’\.
+ New TLS Redis endpoints will be generated when cluster is set to TLS\-preferred mode\. These new endpoints will resolve to the same IPs as the old ones \(non\-TLS\)\.
+ The new TLS Redis configuration endpoint will be exposed in the ElastiCache Console and in the response to `describe-replication-group` API\.

When a cluster is set to ‘transit encryption mode: required’:
+ Old non\-TLS enabled endpoints will be deleted\. There will be no downtime of TLS cluster endpoints\.
+ You can retrieve a new `cluster-configuration-endpoint` from ElastiCache Console or from the `describe-replication-group` API\.

**For CMD clusters with Automatic Failover enabled or Automatic Failover disabled**

When replication group is set to ‘transit encryption mode: preferred’:
+ The original primary endpoint and reader endpoint for none\-TLS enabled cluster will remain active\.
+ New TLS primary and reader endpoints will be generated when cluster is set to TLS `Preferred` mode\. This new endpoints will resolve to the same IP\(s\) as the old ones \(non\-TLS\)\.
+ The new primary endpoint and reader endpoint will be exposed in the ElastiCache Console and in the response to the `describe-replication-group` API\. 

When replication group is set to ‘transit encryption mode: required’:
+ The original primary endpoint and reader endpoint for non\-TLS enabled cluster will remain active\. 
+ Old non\-TLS primary and reader endpoints will be deleted\. There will be no downtime of TLS cluster endpoints\. 
+ You can retrieve new primary and reader endpoints from ElastiCache Console or from the `describe-replication-group` API\.

#### The suggested usage of the DNS records<a name="enable-python-best-practices-before-usage"></a>

**For CME clusters **
+ Use the cluster configuration endpoint instead of per\-node DNS records in your application’s code\. Using per\-node DNS names directly is not recommended because they might change when adding or removing shards\.
+ Don't hardcode cluster configuration endpoint in your application as it will change during this process\. 
+ Having the cluster configuration endpoint hardcoded in your application is a bad practice since it can be changed during this process\. After the in\-transit encryption is completed, query the cluster configuration endpoint with the `describe-replication-group` API \(as demonstrated above \(in bold\)\) and use the DNS you get in response from this point on\.

**For CMD clusters with Automatic Failover enabled **
+ Use the primary endpoint and reader endpoint instead of the per\-node DNS names in your application’s code since the old per\-node DNS names are deleted and new ones are generated when migrating the cluster from no\-TLS to TLS\-preferred\. Using per\-node DNS names directly is not recommended because you might add replicas to your cluster in the future\. Also, when Automatic Failover is enabled, the roles of the primary cluster and replicas are changed automatically by the ElastiCache service, using the primary endpoint and reader endpoint is suggested to help you keep track of those changes\. Lastly, using the reader endpoint will help you distribute your reads from the replicas equally between the replicas in the cluster\.
+ Having the primary endpoint and reader endpoint hardcoded in your application is a bad practice since it can be changed during the TLS migration process\. After the migration change to TLS\-preferred is completed, query the primary endpoint and reader endpoint endpoint with the describe\-replication\-group API and use the DNS you get in response from this point on\. This way you will be able to keep track of changes in endpoints in a dynamic way\.

**For CMD cluster with Automatic Failover disabled **
+ Use the primary endpoint and reader endpoint instead of the per\-node DNS names in your application’s code\. When Automatic Failover is disabled, scaling, patching, failover, and other procedures that are managed automatically by the ElastiCache service when Automatic Failover is enabled are done by you instead\. This makes it easier for you to manually keep track of the different endpoints\. Since the old per\-node DNS names are deleted and new ones are generated when migrating the cluster from no\-TLS to TLS\-preferred, do not use the per\-node DNS names directly\. This is mandatory so that clients will be able to connect to the cluster during the TLS\-migration\. Also, you’ll benefit from evenly spreading the reads between the replicas when using the reader endpoint and keep track of the DNS\-records when adding or deleting replicas form the cluster\.
+ Having the cluster configuration endpoint hardcoded in your application is a bad practice since it can be changed during the TLS migration process\.

### During the in\-transit encryption: pay attention to when the migration process finishes<a name="enable-python-best-practices-during"></a>

Change of transit encryption mode is not immediate and can take some time\. This is especially true for large clusters\. Only when the cluster finishes the migration to TLS\-preferred is it able to accept and serve both TCP and TLS connections\. Therefore, you should not create clients that will try to establish TLS connections to the cluster until the in\-transit encryption is completed\.

There are several ways to get notified when the in\-transit encryption is completed successfully or failed: \(Not shown in the code example above\):
+ Using the SNS service to get a notification when the encryption is completed
+ Using the `describe-events` API that will emit an event when the encryption is completed
+ Seeing a message in the ElastiCache Console that the encryption is completed

You can also implement logic in your application to know if the encryption is completed\. In the example above, we saw several ways to ensure the cluster finishes the migration:
+ Waiting until the migration process starts \(the cluster status changes to “modifying“\), and waiting until the modification is finished \(the cluster status changes back to “available“\)
+ Asserting that the cluster has `transit_encryption_enabled` set to True by querying the `describe-replication-group` API\.

#### After enabling in\-transit encryption: make sure the clients you use are configured properly<a name="enable-python-best-practices-after"></a>

While the cluster is in TLS\-preferred mode, your application should open TLS connections to the cluster and only use those connections\. This way your application will not experience downtime when enabling in\-transit encryption\. You can make sure that there are no clearer TCP connections to the Redis engine using the Redis info command under the SSL section\.

```
# SSL
ssl_enabled:yes
ssl_current_certificate_not_before_date:Mar 20 23:27:07 2017 GMT
ssl_current_certificate_not_after_date:Feb 24 23:27:07 2117 GMT
ssl_current_certificate_serial:D8C7DEA91E684163
tls_mode_connected_tcp_clients:0   (should be zero)
tls_mode_connected_tls_clients:100
```