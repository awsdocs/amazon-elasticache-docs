# JSON\.ARRAPPEND<a name="json-arrappend"></a>

Appends one or more values to the array values at the path\.

Syntax

```
JSON.ARRAPPEND <key> <path> <json> [json ...]
```
+ key \(required\) – A Redis key of JSON document type\.
+ path \(required\) – A JSON path\.
+ json \(required\) – The JSON value to be appended to the array\.

**Return**

If the path is enhanced syntax:
+ Array of integers that represent the new length of the array at each path\.
+ If a value is not an array, its corresponding return value is null\.
+ `SYNTAXERR` error if one of the input json arguments is not a valid JSON string\.
+ `NONEXISTENT` error if the path does not exist\.

If the path is restricted syntax:
+ Integer, the array's new length\.
+ If multiple array values are selected, the command returns the new length of the last updated array\.
+ `WRONGTYPE` error if the value at the path is not an array\.
+ `SYNTAXERR` error if one of the input json arguments is not a valid JSON string\.
+ `NONEXISTENT` error if the path does not exist\.

**Examples**

 Enhanced path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '[[], ["a"], ["a", "b"]]'
OK
127.0.0.1:6379> JSON.ARRAPPEND  k1 $[*] '"c"'
1) (integer) 1
2) (integer) 2
3) (integer) 3
127.0.0.1:6379> JSON.GET k1
"[[\"c\"],[\"a\",\"c\"],[\"a\",\"b\",\"c\"]]"
```

 Restricted path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '[[], ["a"], ["a", "b"]]'
OK
127.0.0.1:6379> JSON.ARRAPPEND  k1 [-1] '"c"'
(integer) 3
127.0.0.1:6379> JSON.GET k1
"[[],[\"a\"],[\"a\",\"b\",\"c\"]]"
```