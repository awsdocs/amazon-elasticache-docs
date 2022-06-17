# JSON\.OBJKEYS<a name="json-objkeys"></a>

Gets key names in the object values at the path\.

Syntax

```
JSON.OBJKEYS <key> [path]
```
+ key \(required\) – A Redis key of JSON document type\.
+ path \(optional\) – A JSON path\. Defaults to the root if not provided\.

**Return**

If the path is enhanced syntax:
+ Array of array of bulk strings\. Each element is an array of keys in a matching object\.
+ If a value is not an object, its corresponding return value is empty value\.
+ Null if the document key does not exist\.

If the path is restricted syntax:
+ Array of bulk strings\. Each element is a key name in the object\.
+ If multiple objects are selected, the command returns the keys of the first object\.
+ `WRONGTYPE` error if the value at the path is not an object\.
+ `WRONGTYPE` error if the path does not exist\.
+ Null if the document key does not exist\.

**Examples**

 Enhanced path syntax:

```
127.0.0.1:6379> JSON.SET k1 $ '{"a":{}, "b":{"a":"a"}, "c":{"a":"a", "b":"bb"}, "d":{"a":1, "b":"b", "c":{"a":3,"b":4}}, "e":1}'
OK
127.0.0.1:6379> JSON.OBJKEYS k1 $.*
1) (empty array)
2) 1) "a"
3) 1) "a"
   2) "b"
4) 1) "a"
   2) "b"
   3) "c"
5) (empty array)
127.0.0.1:6379> JSON.OBJKEYS k1 $.d
1) 1) "a"
   2) "b"
   3) "c"
```

 Restricted path syntax:

```
127.0.0.1:6379> JSON.SET k1 $ '{"a":{}, "b":{"a":"a"}, "c":{"a":"a", "b":"bb"}, "d":{"a":1, "b":"b", "c":{"a":3,"b":4}}, "e":1}'
OK
127.0.0.1:6379> JSON.OBJKEYS k1 .*
1) "a"
127.0.0.1:6379> JSON.OBJKEYS k1 .d
1) "a"
2) "b"
3) "c"
```