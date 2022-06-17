# JSON\.SET<a name="json-set"></a>

Sets JSON values at the path\.

If the path calls for an object member:
+ If the parent element does not exist, the command returns a NONEXISTENT error\.
+ If the parent element exists but is not an object, the command returns ERROR\.
+ If the parent element exists and is an object:
  +  If the member does not exist, a new member will be appended to the parent object if and only if the parent object is the last child in the path\. Otherwise, the command returns a NONEXISTENT error\.
  +  If the member exists, its value will be replaced by the JSON value\.

If the path calls for an array index:
+ If the parent element does not exist, the command returns a NONEXISTENT error\.
+ If the parent element exists but is not an array, the command returns ERROR\.
+ If the parent element exists but the index is out of bounds, the command returns an OUTOFBOUNDARIES error\.
+ If the parent element exists and the index is valid, the element will be replaced by the new JSON value\.

If the path calls for an object or array, the value \(object or array\) will be replaced by the new JSON value\.

Syntax

```
JSON.SET <key> <path> <json> [NX | XX] 
```

\[NX \| XX\] Where you can have 0 or 1 of \[NX \| XX\] identifiers\.
+ key \(required\) – A Redis key of JSON document type\.
+ path \(required\) – A JSON path\. For a new Redis key, the JSON path must be the root "\."\.
+ NX \(optional\) – If the path is the root, set the value only if the Redis key does not exist\. That is, insert a new document\. If the path is not the root, set the value only if the path does not exist\. That is, insert a value into the document\.
+ XX \(optional\) – If the path is the root, set the value only if the Redis key exists\. That is, replace the existing document\. If the path is not the root, set the value only if the path exists\. That is, update the existing value\.

**Return**
+ Simple String 'OK' on success\.
+ Null if the NX or XX condition is not met\.

**Examples**

 Enhanced path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '{"a":{"a":1, "b":2, "c":3}}'
OK
127.0.0.1:6379> JSON.SET k1 $.a.* '0'
OK
127.0.0.1:6379> JSON.GET k1
"{\"a\":{\"a\":0,\"b\":0,\"c\":0}}"

127.0.0.1:6379> JSON.SET k2 . '{"a": [1,2,3,4,5]}'
OK
127.0.0.1:6379> JSON.SET k2 $.a[*] '0'
OK
127.0.0.1:6379> JSON.GET k2
"{\"a\":[0,0,0,0,0]}"
```

 Restricted path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '{"c":{"a":1, "b":2}, "e": [1,2,3,4,5]}'
OK
127.0.0.1:6379> JSON.SET k1 .c.a '0'
OK
127.0.0.1:6379> JSON.GET k1
"{\"c\":{\"a\":0,\"b\":2},\"e\":[1,2,3,4,5]}"
127.0.0.1:6379> JSON.SET k1 .e[-1] '0'
OK
127.0.0.1:6379> JSON.GET k1
"{\"c\":{\"a\":0,\"b\":2},\"e\":[1,2,3,4,0]}"
127.0.0.1:6379> JSON.SET k1 .e[5] '0'
(error) OUTOFBOUNDARIES Array index is out of bounds
```