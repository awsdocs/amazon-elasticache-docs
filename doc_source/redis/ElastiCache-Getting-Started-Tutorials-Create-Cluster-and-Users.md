# Creating Elasticache clusters and users<a name="ElastiCache-Getting-Started-Tutorials-Create-Cluster-and-Users"></a>

The following examples use the boto3 SDK for ElastiCache management operations \(cluster or user creation\) and redis\-py/redis\-py\-cluster for data handling\.

**Topics**
+ [Create a cluster mode disabled cluster](#ElastiCache-Getting-Started-Tutorials-Create-Cluster)
+ [Create a cluster mode disabled cluster with TLS and RBAC](#ElastiCache-Getting-Started-Tutorials-RBAC)
+ [Create a cluster mode enabled cluster](#ElastiCache-Getting-Started-Tutorials-Cluster-Enabled)
+ [Create a cluster mode enabled cluster with TLS and RBAC](#ElastiCache-Getting-Started-Tutorials-Cluster-RBAC)
+ [Check if users/usergroup exists, otherwise create them](#ElastiCache-Getting-Started-Tutorials-Users)

## Create a cluster mode disabled cluster<a name="ElastiCache-Getting-Started-Tutorials-Create-Cluster"></a>

Copy the following program and paste it into a file named *CreateClusterModeDisabledCluster\.py*\.

```
import boto3
import logging

logging.basicConfig(level=logging.INFO)
client = boto3.client('elasticache')

def create_cluster_mode_disabled(CacheNodeType='cache.t3.small',EngineVersion='6.x',NumCacheClusters=2,ReplicationGroupDescription='Sample cache cluster',ReplicationGroupId=None):
    """Creates an Elasticache Cluster with cluster mode disabled

    Returns a dictionary with the API response

    :param CacheNodeType: Node type used on the cluster. If not specified, cache.t3.small will be used
    Refer to https://docs..amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html for supported node types
    :param EngineVersion: Engine version to be used. If not specified, Redis 6.x will be used.
    :param NumCacheClusters: Number of nodes in the cluster. Minimum 1 (just a primary node) and maximun 6 (1 primary and 5 replicas).
    If not specified, cluster will be created with 1 primary and 1 replica.
    :param ReplicationGroupDescription: Description for the cluster.
    :param ReplicationGroupId: Name for the cluster
    :return: dictionary with the API results

    """
    if not ReplicationGroupId:
        return 'ReplicationGroupId parameter is required'

    response = client.create_replication_group(
        AutomaticFailoverEnabled=True,
        CacheNodeType=CacheNodeType,
        Engine='redis',
        EngineVersion=EngineVersion,
        NumCacheClusters=NumCacheClusters,
        ReplicationGroupDescription=ReplicationGroupDescription,
        ReplicationGroupId=ReplicationGroupId,
        SnapshotRetentionLimit=30,
    )
    return response


if __name__ == '__main__':
    
    # Creates an Elasticace Cluster mode disabled cluster, based on cache.m6g.large nodes, Redis 6, one primary and two replicas
    elasticacheResponse = create_cluster_mode_disabled(
        #CacheNodeType='cache.m6g.large', 
        EngineVersion='6.x',
        NumCacheClusters=3,
        ReplicationGroupDescription='Redis cluster mode disabled with replicas',
        ReplicationGroupId='redis202104053'
        )
    
    logging.info(elasticacheResponse)
```

To run the program, enter the following command:

 `python CreateClusterModeDisabledCluster.py`

For more information, see [Managing clusters](Clusters.md)\.

## Create a cluster mode disabled cluster with TLS and RBAC<a name="ElastiCache-Getting-Started-Tutorials-RBAC"></a>

To ensure security, you can use Transport Layer Security \(TLS\) and Role\-Based Access Control \(RBAC\) when creating a cluster mode disabled cluster\. Unlike Redis AUTH, where all authenticated clients have full replication group access if their token is authenticated, RBAC enables you to control cluster access through user groups\. These user groups are designed as a way to organize access to replication groups\. For more information, see [Authenticating users with Role\-Based Access Control \(RBAC\)](Clusters.RBAC.md)\.

Copy the following program and paste it into a file named *ClusterModeDisabledWithRBAC\.py*\.

```
import boto3
import logging

logging.basicConfig(level=logging.INFO)
client = boto3.client('elasticache')

def create_cluster_mode_disabled_rbac(CacheNodeType='cache.t3.small',EngineVersion='6.x',NumCacheClusters=2,ReplicationGroupDescription='Sample cache cluster',ReplicationGroupId=None, UserGroupIds=None, SecurityGroupIds=None,CacheSubnetGroupName=None):
    """Creates an Elasticache Cluster with cluster mode disabled and RBAC

    Returns a dictionary with the API response

    :param CacheNodeType: Node type used on the cluster. If not specified, cache.t3.small will be used
    Refer to https://docs..amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html for supported node types
    :param EngineVersion: Engine version to be used. If not specified, Redis 6.x will be used.
    :param NumCacheClusters: Number of nodes in the cluster. Minimum 1 (just a primary node) and maximun 6 (1 primary and 5 replicas).
    If not specified, cluster will be created with 1 primary and 1 replica.
    :param ReplicationGroupDescription: Description for the cluster.
    :param ReplicationGroupId: Mandatory name for the cluster.
    :param UserGroupIds: Mandatory list of user groups to be assigned to the cluster.
    :param SecurityGroupIds: List of security groups to be assigned. If not defined, default will be used
    :param CacheSubnetGroupName: subnet group where the cluster will be placed. If not defined, default will be used.
    :return: dictionary with the API results

    """
    if not ReplicationGroupId:
        return {'Error': 'ReplicationGroupId parameter is required'}
    elif not isinstance(UserGroupIds,(list)):
        return {'Error': 'UserGroupIds parameter is required and must be a list'}

    params={'AutomaticFailoverEnabled': True, 
            'CacheNodeType': CacheNodeType, 
            'Engine': 'redis', 
            'EngineVersion': EngineVersion, 
            'NumCacheClusters': NumCacheClusters, 
            'ReplicationGroupDescription': ReplicationGroupDescription, 
            'ReplicationGroupId': ReplicationGroupId, 
            'SnapshotRetentionLimit': 30, 
            'TransitEncryptionEnabled': True, 
            'UserGroupIds':UserGroupIds
        }

    # defaults will be used if CacheSubnetGroupName or SecurityGroups are not explicit.
    if isinstance(SecurityGroupIds,(list)):
        params.update({'SecurityGroupIds':SecurityGroupIds})
    if CacheSubnetGroupName:
        params.update({'CacheSubnetGroupName':CacheSubnetGroupName})

    response = client.create_replication_group(**params)
    return response

if __name__ == '__main__':

    # Creates an Elasticace Cluster mode disabled cluster, based on cache.m6g.large nodes, Redis 6, one primary and two replicas.
    # Assigns the existent user group "mygroup" for RBAC authentication
   
    response=create_cluster_mode_disabled_rbac(
        CacheNodeType='cache.m6g.large',
        EngineVersion='6.x',
        NumCacheClusters=3,
        ReplicationGroupDescription='Redis cluster mode disabled with replicas',
        ReplicationGroupId='redis202104',
        UserGroupIds=[
            'mygroup'
        ],
        SecurityGroupIds=[
            'sg-7cc73803'
        ],
        CacheSubnetGroupName='default'
    )

    logging.info(response)
```

To run the program, enter the following command:

 `python ClusterModeDisabledWithRBAC.py`

For more information, see [Managing clusters](Clusters.md)\.

## Create a cluster mode enabled cluster<a name="ElastiCache-Getting-Started-Tutorials-Cluster-Enabled"></a>

Copy the following program and paste it into a file named *ClusterModeEnabled\.py*\.

```
import boto3
import logging

logging.basicConfig(level=logging.INFO)
client = boto3.client('elasticache')

def create_cluster_mode_enabled(CacheNodeType='cache.t3.small',EngineVersion='6.x',NumNodeGroups=1,ReplicasPerNodeGroup=1, ReplicationGroupDescription='Sample cache with cluster mode enabled',ReplicationGroupId=None):
    """Creates an Elasticache Cluster with cluster mode enabled

    Returns a dictionary with the API response

    :param CacheNodeType: Node type used on the cluster. If not specified, cache.t3.small will be used
    Refer to https://docs..amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html for supported node types
    :param EngineVersion: Engine version to be used. If not specified, Redis 6.x will be used.
    :param NumNodeGroups: Number of shards in the cluster. Minimum 1 and maximun 90.
    If not specified, cluster will be created with 1 shard.
    :param ReplicasPerNodeGroup: Number of replicas per shard. If not specified 1 replica per shard will be created.
    :param ReplicationGroupDescription: Description for the cluster.
    :param ReplicationGroupId: Name for the cluster
    :return: dictionary with the API results

    """
    if not ReplicationGroupId:
        return 'ReplicationGroupId parameter is required'
    
    response = client.create_replication_group(
        AutomaticFailoverEnabled=True,
        CacheNodeType=CacheNodeType,
        Engine='redis',
        EngineVersion=EngineVersion,
        ReplicationGroupDescription=ReplicationGroupDescription,
        ReplicationGroupId=ReplicationGroupId,
    #   Creates a cluster mode enabled cluster with 1 shard(NumNodeGroups), 1 primary node (implicit) and 2 replicas (replicasPerNodeGroup)
        NumNodeGroups=NumNodeGroups,
        ReplicasPerNodeGroup=ReplicasPerNodeGroup,
        CacheParameterGroupName='default.redis6.x.cluster.on'
    )

    return response


# Creates a cluster mode enabled 
response = create_cluster_mode_enabled(
    CacheNodeType='cache.m6g.large',
    EngineVersion='6.x',
    ReplicationGroupDescription='Redis cluster mode enabled with replicas',
    ReplicationGroupId='redis20210',
#   Creates a cluster mode enabled cluster with 1 shard(NumNodeGroups), 1 primary (implicit) and 2 replicas (replicasPerNodeGroup)
    NumNodeGroups=2,
    ReplicasPerNodeGroup=1,
)

logging.info(response)
```

To run the program, enter the following command:

 `python ClusterModeEnabled.py`

For more information, see [Managing clusters](Clusters.md)\.

## Create a cluster mode enabled cluster with TLS and RBAC<a name="ElastiCache-Getting-Started-Tutorials-Cluster-RBAC"></a>

To ensure security, you can use Transport Layer Security \(TLS\) and Role\-Based Access Control \(RBAC\) when creating a cluster mode enabled cluster\. Unlike Redis AUTH, where all authenticated clients have full replication group access if their token is authenticated, RBAC enables you to control cluster access through user groups\. These user groups are designed as a way to organize access to replication groups\. For more information, see [Authenticating users with Role\-Based Access Control \(RBAC\)](Clusters.RBAC.md)\.

Copy the following program and paste it into a file named *ClusterModeEnabledWithRBAC\.py*\.

```
import boto3
import logging

logging.basicConfig(level=logging.INFO)
client = boto3.client('elasticache')

def create_cluster_mode_enabled(CacheNodeType='cache.t3.small',EngineVersion='6.x',NumNodeGroups=1,ReplicasPerNodeGroup=1, ReplicationGroupDescription='Sample cache with cluster mode enabled',ReplicationGroupId=None,UserGroupIds=None, SecurityGroupIds=None,CacheSubnetGroupName=None,CacheParameterGroupName='default.redis6.x.cluster.on'):
    """Creates an Elasticache Cluster with cluster mode enabled and RBAC

    Returns a dictionary with the API response

    :param CacheNodeType: Node type used on the cluster. If not specified, cache.t3.small will be used
    Refer to https://docs..amazon.com/AmazonElastiCache/latest/red-ug/CacheNodes.SupportedTypes.html for supported node types
    :param EngineVersion: Engine version to be used. If not specified, Redis 6.x will be used.
    :param NumNodeGroups: Number of shards in the cluster. Minimum 1 and maximun 90.
    If not specified, cluster will be created with 1 shard.
    :param ReplicasPerNodeGroup: Number of replicas per shard. If not specified 1 replica per shard will be created.
    :param ReplicationGroupDescription: Description for the cluster.
    :param ReplicationGroupId: Name for the cluster.
    :param CacheParameterGroupName: Parameter group to be used. Must be compatible with the engine version and cluster mode enabled.
    :return: dictionary with the API results

    """
    if not ReplicationGroupId:
        return 'ReplicationGroupId parameter is required'
    elif not isinstance(UserGroupIds,(list)):
        return {'Error': 'UserGroupIds parameter is required and must be a list'}

    params={'AutomaticFailoverEnabled': True, 
            'CacheNodeType': CacheNodeType, 
            'Engine': 'redis', 
            'EngineVersion': EngineVersion, 
            'ReplicationGroupDescription': ReplicationGroupDescription, 
            'ReplicationGroupId': ReplicationGroupId, 
            'SnapshotRetentionLimit': 30, 
            'TransitEncryptionEnabled': True, 
            'UserGroupIds':UserGroupIds,
            'NumNodeGroups': NumNodeGroups,
            'ReplicasPerNodeGroup': ReplicasPerNodeGroup,
            'CacheParameterGroupName': CacheParameterGroupName
        }

    # defaults will be used if CacheSubnetGroupName or SecurityGroups are not explicit.
    if isinstance(SecurityGroupIds,(list)):
        params.update({'SecurityGroupIds':SecurityGroupIds})
    if CacheSubnetGroupName:
        params.update({'CacheSubnetGroupName':CacheSubnetGroupName})

    response = client.create_replication_group(**params)
    return response

if __name__ == '__main__':
    # Creates a cluster mode enabled cluster
    response = create_cluster_mode_enabled(
        CacheNodeType='cache.m6g.large',
        EngineVersion='6.x',
        ReplicationGroupDescription='Redis cluster mode enabled with replicas',
        ReplicationGroupId='redis2021',
    #   Creates a cluster mode enabled cluster with 1 shard(NumNodeGroups), 1 primary (implicit) and 2 replicas (replicasPerNodeGroup)
        NumNodeGroups=2,
        ReplicasPerNodeGroup=1,
        UserGroupIds=[
            'mygroup'
        ],
        SecurityGroupIds=[
            'sg-7cc73803'
        ],
        CacheSubnetGroupName='default'

    )
    
    logging.info(response)
```

To run the program, enter the following command:

 `python ClusterModeEnabledWithRBAC.py`

For more information, see [Managing clusters](Clusters.md)\.

## Check if users/usergroup exists, otherwise create them<a name="ElastiCache-Getting-Started-Tutorials-Users"></a>

With RBAC, you create users and assign them specific permissions by using an access string\. You assign the users to user groups aligned with a specific role \(administrators, human resources\) that are then deployed to one or more ElastiCache for Redis replication groups\. By doing this, you can establish security boundaries between clients using the same Redis replication group or groups and prevent clients from accessing each other’s data\. For more information, see [Authenticating users with Role\-Based Access Control \(RBAC\)](Clusters.RBAC.md)\.

Copy the following program and paste it into a file named *UserAndUserGroups\.py*\.

```
import boto3
import logging

logging.basicConfig(level=logging.INFO)
client = boto3.client('elasticache')

def check_user_exists(UserId):
    """Checks if UserId exists

    Returns True if UserId exists, otherwise False
    :param UserId: Elasticache User ID
    :return: True|False
    """
    try:
        response = client.describe_users(
            UserId=UserId,
        )
        if response['Users'][0]['UserId'].lower() == UserId.lower():
            return True
    except Exception as e:
        if e.response['Error']['Code'] == 'UserNotFound':
            logging.info(e.response['Error'])
            return False
        else:
            raise

def check_group_exists(UserGroupId):
    """Checks if UserGroupID exists

    Returns True if Group ID exists, otherwise False
    :param UserGroupId: Elasticache User ID
    :return: True|False
    """

    try:
        response = client.describe_user_groups(
            UserGroupId=UserGroupId
        )
        if response['UserGroups'][0]['UserGroupId'].lower() == UserGroupId.lower():
            return True
    except Exception as e:
        if e.response['Error']['Code'] == 'UserGroupNotFound':
            logging.info(e.response['Error'])
            return False
        else:
            raise

def create_user(UserId=None,UserName=None,Password=None,AccessString=None):
    """Creates a new user

    Returns the ARN for the newly created user or the error message
    :param UserId: Elasticache user ID. User IDs must be unique
    :param UserName: Elasticache user name. Elasticache allows multiple users with the same name as long as the associated user ID is unique.
    :param Password: Password for user. Must have at least 16 chars.
    :param AccessString: Access string with the permissions for the user. For details refer to https://docs..amazon.com/AmazonElastiCache/latest/red-ug/Clusters.RBAC.html#Access-string
    :return: user ARN
    """
    try:
        response = client.create_user(
            UserId=UserId,
            UserName=UserName,
            Engine='Redis',
            Passwords=[Password],
            AccessString=AccessString,
            NoPasswordRequired=False
        )
        return response['ARN']
    except Exception as e:
        logging.info(e.response['Error'])
        return e.response['Error']

def create_group(UserGroupId=None, UserIds=None):
    """Creates a new group.
    A default user is required (mandatory) and should be specified in the UserIds list

    Return: Group ARN
    :param UserIds: List with user IDs to be associated with the new group. A default user is required
    :param UserGroupId: The ID (name) for the group
    :return: Group ARN
    """
    try:
        response = client.create_user_group(
            UserGroupId=UserGroupId,
            Engine='Redis',
            UserIds=UserIds
        )
        return response['ARN']
    except Exception as e:
        logging.info(e.response['Error'])


if __name__ == '__main__':
    
    groupName='mygroup2'
    userName = 'myuser2'
    userId=groupName+'-'+userName

    # Creates a new user if the user ID does not exist.
    for tmpUserId,tmpUserName in [ (userId,userName), (groupName+'-default','default')]:
        if not check_user_exists(tmpUserId):
            response=create_user(UserId=tmpUserId, UserName=tmpUserName,Password='MyStrongPasswordWithNumbers',AccessString='on ~* +@all')
            logging.info(response)
        # assigns the new user ID to the user group
    if not check_group_exists(groupName):
        UserIds = [ userId , groupName+'-default']
        response=create_group(UserGroupId=groupName,UserIds=UserIds)
        logging.info(response)
```

To run the program, enter the following command:

 `python UserAndUserGroups.py`