# JSON\.ARRINSERT<a name="json-arrinsert"></a>

Inserts one or more values into the array values at the path before the index\.

Syntax

```
JSON.ARRINSERT <key> <path> <index> <json> [json ...]
```
+ key \(required\) – A Redis key of JSON document type\.
+ path \(required\) – A JSON path\.
+ index \(required\) – An array index before which values are inserted\.
+ json \(required\) – The JSON value to be appended to the array\.

**Return**

If the path is enhanced syntax:
+ Array of integers that represent the new length of the array at each path\.
+ If a value is an empty array, its corresponding return value is null\.
+ If a value is not an array, its corresponding return value is null\.
+ `OUTOFBOUNDARIES` error if the index argument is out of bounds\.

If the path is restricted syntax:
+ Integer, the new length of the array\.
+ `WRONGTYPE` error if the value at the path is not an array\.
+ `OUTOFBOUNDARIES` error if the index argument is out of bounds\.

**Examples**

 Enhanced path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '[[], ["a"], ["a", "b"]]'
OK
127.0.0.1:6379> JSON.ARRINSERT k1 $[*] 0 '"c"'
1) (integer) 1
2) (integer) 2
3) (integer) 3
127.0.0.1:6379> JSON.GET k1
"[[\"c\"],[\"c\",\"a\"],[\"c\",\"a\",\"b\"]]"
```

 Restricted path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '[[], ["a"], ["a", "b"]]'
OK
127.0.0.1:6379> JSON.ARRINSERT k1 . 0 '"c"'
(integer) 4
127.0.0.1:6379> JSON.GET k1
"[\"c\",[],[\"a\"],[\"a\",\"b\"]]"
```