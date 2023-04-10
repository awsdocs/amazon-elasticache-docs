# Step 2\.1: Create the deployment package<a name="Lambda.CreateLambdaFunction.DeploymentPkg"></a>

Currently the example code for the Lambda function is only supplied in Python\.

## Python<a name="Lambda.CreateLambdaFunction.DeploymentPkg.Python"></a>

The following example Python code reads and writes an item to your \#ELC; cluster\. Copy the code and save it into a file called app\.py\. 

```
from __future__ import print_function
import time
import uuid
import sys
import socket
import elasticache_auto_discovery
from pymemcache.client.hash import HashClient

# ElastiCache settings
elasticache_config_endpoint = "your-elasticache-cluster-endpoint:port"
nodes = elasticache_auto_discovery.discover(elasticache_config_endpoint)
nodes = map(lambda x: (x[1], int(x[2])), nodes)
memcache_client = HashClient(nodes)

######
# This function puts into memcache and get from it.
# Memcached is hosted using elasticache
######
def handler(event, context):

    # Create a random UUID... this will be the sample element we add to the cache.
    uuid_in = uuid.uuid4().hex
    
    # Put the UUID to the cache.
    memcache_client.set('uuid', uuid_in)
    
    # Get the item (UUID) from the cache.
    uuid_out = memcache_client.get('uuid')
    
    # Print the results
    if uuid_out == uuid_in:
        # this print should see the CloudWatch Logs and Lambda console.
        print "Success: Inserted: %s. Fetched %s from memcache." %(uuid_in, uuid_out)
    else:
        raise Exception("Bad value retrieved :(. Expected %s got %s." %(uuid_in, uuid_out)) 

    return "Fetched value from Memcached"
```

The preceeding code depends upon the `pymemcache` and `elasticache-auto-discovery` libraries\. Install these libraries using pip\.
+  `pymemcache`—The Lambda function code uses this library \(see [pymemcache](https://pypi.python.org/pypi/pymemcache)\) to create a `HashClient` object which sets and gets items from Memcached\.
+  `elasticache-auto-discovery`—The Lambda function uses this library \(see [elasticache\-auto\-discovery](https://pypi.python.org/pypi/elasticache-auto-discovery)\) to get the nodes in your Amazon ElastiCache cluster\. 

Save the preceding Python code in a file named `app.py`\. Then zip all of these files into a file named app\.zip to create your deployment package\. For step\-by\-step instructions, see [Creating a Deployment Package \(Python\)](https://docs.aws.amazon.com//lambda-dg-vpc/lambda-python-how-to-create-deployment-package.html)\.

**Next Step**

[Step 2\.2: Create the IAM role \(execution role\)](Lambda.CreateLambdaFunction.IAMRole.md)