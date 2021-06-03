# Usage examples<a name="ElastiCache-Getting-Started-Tutorials-Usage"></a>

The following examples use the boto3 SDK for ElastiCache to work with ElastiCache\.

**Topics**
+ [Set and Get strings](#ElastiCache-Getting-Started-Tutorials-set-strings)
+ [Set and Get a hash with multiple items](#ElastiCache-Getting-Started-Tutorials-set-hash)
+ [Publish \(write\) and subscribe \(read\) from a Pub/Sub channel](#ElastiCache-Getting-Started-Tutorials-pub-sub)
+ [Write and read from a stream](#ElastiCache-Getting-Started-Tutorials-read-write-stream)

## Set and Get strings<a name="ElastiCache-Getting-Started-Tutorials-set-strings"></a>

Copy the following program and paste it into a file named *SetAndGetStrings\.py*\.

```
import time
import logging
logging.basicConfig(level=logging.INFO,format='%(asctime)s: %(message)s')

keyName='mykey'
currTime=time.ctime(time.time())

# Set the key 'mykey' with the current date and time as value. 
# The Key will expire and removed from cache in 60 seconds.
redis.set(keyName, currTime, ex=60)

# Sleep just for better illustration of TTL (expiration) value
time.sleep(5)

# Retrieve the key value and current TTL
keyValue=redis.get(keyName)
keyTTL=redis.ttl(keyName)

logging.info("Key {} was set at {} and has {} seconds until expired".format(keyName, keyValue, keyTTL))
```

To run the program, enter the following command:

 `python SetAndGetStrings.py`

## Set and Get a hash with multiple items<a name="ElastiCache-Getting-Started-Tutorials-set-hash"></a>

Copy the following program and paste it into a file named *SetAndGetHash\.py*\.

```
import logging
import time

logging.basicConfig(level=logging.INFO,format='%(asctime)s: %(message)s')

keyName='mykey'
keyValues={'datetime': time.ctime(time.time()), 'epochtime': time.time()}

# Set the hash 'mykey' with the current date and time in human readable format (datetime field) and epoch number (epochtime field). 
redis.hset(keyName, mapping=keyValues)

# Set the key to expire and removed from cache in 60 seconds.
redis.expire(keyName, 60)

# Sleep just for better illustration of TTL (expiration) value
time.sleep(5)

# Retrieves all the fields and current TTL
keyValues=redis.hgetall(keyName)
keyTTL=redis.ttl(keyName)

logging.info("Key {} was set at {} and has {} seconds until expired".format(keyName, keyValues, keyTTL))
```

To run the program, enter the following command:

 `python SetAndGetHash.py`

## Publish \(write\) and subscribe \(read\) from a Pub/Sub channel<a name="ElastiCache-Getting-Started-Tutorials-pub-sub"></a>

Copy the following program and paste it into a file named *PubAndSub\.py*\.

```
import logging
import time

def handlerFunction(message):
    """Prints message got from PubSub channel to the log output

    Return None
    :param message: message to log
    """
    logging.info(message)

logging.basicConfig(level=logging.INFO)
redis = Redis(host="redis202104053.tihewd.ng.0001.use1.cache.amazonaws.com", port=6379, decode_responses=True)


# Creates the subscriber connection on "mychannel"
subscriber = redis.pubsub()
subscriber.subscribe(**{'mychannel': handlerFunction})

# Creates a new thread to watch for messages while the main process continues with its routines
thread = subscriber.run_in_thread(sleep_time=0.01)

# Creates publisher connection on "mychannel"
redis.publish('mychannel', 'My message')

# Publishes several messages. Subscriber thread will read and print on log.
while True:
    redis.publish('mychannel',time.ctime(time.time()))
    time.sleep(1)
```

To run the program, enter the following command:

 `python PubAndSub.py`

## Write and read from a stream<a name="ElastiCache-Getting-Started-Tutorials-read-write-stream"></a>

Copy the following program and paste it into a file named *ReadWriteStream\.py*\.

```
from redis import Redis
import redis.exceptions as exceptions
import logging
import time
import threading

logging.basicConfig(level=logging.INFO)

def writeMessage(streamName):
    """Starts a loop writting the current time and thread name to 'streamName'

    :param streamName: Stream (key) name to write messages.
    """
    fieldsDict={'writerId':threading.currentThread().getName(),'myvalue':None}
    while True:
        fieldsDict['myvalue'] = time.ctime(time.time())
        redis.xadd(streamName,fieldsDict)
        time.sleep(1)

def readMessage(groupName=None,streamName=None):
    """Starts a loop reading from 'streamName'
    Multiple threads will read from the same stream consumer group. Consumer group is used to coordinate data distribution.
    Once a thread acknowleges the message, it won't be provided again. If message wasn't acknowledged, it can be served to another thread.

    :param groupName: stream group were multiple threads will read.
    :param streamName: Stream (key) name where messages will be read.
    """

    readerID=threading.currentThread().getName()
    while True:
        try:
            # Check if the stream has any message
            if redis.xlen(streamName)>0:
                # Check if if the messages are new (not acknowledged) or not (already processed)
                streamData=redis.xreadgroup(groupName,readerID,{streamName:'>'},count=1)
                if len(streamData) > 0:
                    msgId,message = streamData[0][1][0]
                    logging.info("{}: Got {} from ID {}".format(readerID,message,msgId))
                    #Do some processing here. If the message has been processed sucessfuly, acknowledge it and (optional) delete the message.
                    redis.xack(streamName,groupName,msgId)
                    logging.info("Stream message ID {} read and processed successfuly by {}".format(msgId,readerID))
                    redis.xdel(streamName,msgId)
            else:
                pass
        except:
            raise
            
        time.sleep(0.5)

# Creates the stream 'mystream' and consumer group 'myworkergroup' where multiple threads will write/read.
try:
    redis.xgroup_create('mystream','myworkergroup',mkstream=True)
except exceptions.ResponseError as e:
    logging.info("Consumer group already exists. Will continue despite the error: {}".format(e))
except:
    raise

# Starts 5 writer threads.
for writer_no in range(5):
    writerThread = threading.Thread(target=writeMessage, name='writer-'+str(writer_no), args=('mystream',),daemon=True)
    writerThread.start()

# Starts 10 reader threads
for reader_no in range(10):
    readerThread = threading.Thread(target=readMessage, name='reader-'+str(reader_no), args=('myworkergroup','mystream',),daemon=True)
    readerThread.daemon = True
    readerThread.start()

# Keep the code running for 30 seconds
time.sleep(30)
```

To run the program, enter the following command:

 `python ReadWriteStream.py`