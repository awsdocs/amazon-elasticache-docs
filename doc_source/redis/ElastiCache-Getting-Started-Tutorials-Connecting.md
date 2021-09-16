# Connecting to Elasticache<a name="ElastiCache-Getting-Started-Tutorials-Connecting"></a>

The following examples use the Redis client to connect to ElastiCache\.

**Topics**
+ [Connecting to a cluster mode disabled cluster](#ElastiCache-Getting-Started-Tutorials-Connecting-cluster-mode-disabled)
+ [Connecting to a cluster mode enabled cluster](#ElastiCache-Getting-Started-Tutorials-Connecting-cluster-mode-enabled)

## Connecting to a cluster mode disabled cluster<a name="ElastiCache-Getting-Started-Tutorials-Connecting-cluster-mode-disabled"></a>

Copy the following program and paste it into a file named *ConnectClusterModeDisabled\.py*\.

```
from redis import Redis
import logging

logging.basicConfig(level=logging.INFO)
redis = Redis(host='primary.xxx.yyyyyy.zzz1.cache.amazonaws.com', port=6379, decode_responses=True, ssl=True, username='myuser', password='MyPassword0123456789')

if redis.ping():
    logging.info("Connected to Redis")
```

To run the program, enter the following command:

 `python ConnectClusterModeDisabled.py`

## Connecting to a cluster mode enabled cluster<a name="ElastiCache-Getting-Started-Tutorials-Connecting-cluster-mode-enabled"></a>

Copy the following program and paste it into a file named *ConnectClusterModeEnabled\.py*\.

```
from rediscluster import RedisCluster
import logging

logging.basicConfig(level=logging.INFO)
redis = RedisCluster(startup_nodes=[{"host": "xxx.yyy.clustercfg.zzz1.cache.amazonaws.com","port": "6379"}], decode_responses=True,skip_full_coverage_check=True)

if redis.ping():
    logging.info("Connected to Redis")
```

To run the program, enter the following command:

 `python ConnectClusterModeEnabled.py`