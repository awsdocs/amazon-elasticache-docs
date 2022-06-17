# JSON\.TOGGLE<a name="json-toggle"></a>

Toggles Boolean values between true and false at the path\.

Syntax

```
JSON.TOGGLE <key> [path] 
```
+ key \(required\) – A Redis key of JSON document type\.
+ path \(optional\) – A JSON path\. Defaults to the root if not provided\.

**Return**

If the path is enhanced syntax:
+ Array of integers \(0 \- false, 1 \- true\) that represent the resulting Boolean value at each path\.
+ If a value is a not a Boolean value, its corresponding return value is null\.
+ `NONEXISTENT` if the document key does not exist\.

If the path is restricted syntax:
+ String \("true"/"false"\) that represents the resulting Boolean value\.
+ `NONEXISTENT` if the document key does not exist\.
+ `WRONGTYPE` error if the value at the path is not a Boolean value\.

**Examples**

 Enhanced path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '{"a":true, "b":false, "c":1, "d":null, "e":"foo", "f":[], "g":{}}'
OK
127.0.0.1:6379> JSON.TOGGLE k1 $.*
1) (integer) 0
2) (integer) 1
3) (nil)
4) (nil)
5) (nil)
6) (nil)
7) (nil)
127.0.0.1:6379> JSON.TOGGLE k1 $.*
1) (integer) 1
2) (integer) 0
3) (nil)
4) (nil)
5) (nil)
6) (nil)
7) (nil)
```

 Restricted path syntax:

```
127.0.0.1:6379> JSON.SET k1 . true
OK
127.0.0.1:6379> JSON.TOGGLE k1
"false"
127.0.0.1:6379> JSON.TOGGLE k1
"true"

127.0.0.1:6379> JSON.SET k2 . '{"isAvailable": false}'
OK
127.0.0.1:6379> JSON.TOGGLE k2 .isAvailable
"true"
127.0.0.1:6379> JSON.TOGGLE k2 .isAvailable
"false"
```