# JSON\.ARRLEN<a name="json-arrlen"></a>

Gets the length of the array values at the path\.

Syntax

```
JSON.ARRLEN <key> [path] 
```
+ key \(required\) – A Redis key of JSON document type\.
+ path \(optional\) – A JSON path\. Defaults to the root if not provided\.

**Return**

If the path is enhanced syntax:
+ Array of integers that represent the array length at each path\.
+ If a value is not an array, its corresponding return value is null\.
+ Null if the document key does not exist\.

If the path is restricted syntax:
+ Array of bulk strings\. Each element is a key name in the object\.
+ Integer, array length\.
+ If multiple objects are selected, the command returns the first array's length\.
+ `WRONGTYPE` error if the value at the path is not an array\.
+ `WRONGTYPE` error if the path does not exist\.
+ Null if the document key does not exist\.

**Examples**

 Enhanced path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '[[], [\"a\"], [\"a\", \"b\"], [\"a\", \"b\", \"c\"]]'
(error) SYNTAXERR Failed to parse JSON string due to syntax error
127.0.0.1:6379> JSON.SET k1 . '[[], ["a"], ["a", "b"], ["a", "b", "c"]]'
OK
127.0.0.1:6379> JSON.ARRLEN k1 $[*]
1) (integer) 0
2) (integer) 1
3) (integer) 2
4) (integer) 3

127.0.0.1:6379> JSON.SET k2 . '[[], "a", ["a", "b"], ["a", "b", "c"], 4]'
OK
127.0.0.1:6379> JSON.ARRLEN k2 $[*]
1) (integer) 0
2) (nil)
3) (integer) 2
4) (integer) 3
5) (nil)
```

 Restricted path syntax:

```
127.0.0.1:6379> JSON.SET k1 . '[[], ["a"], ["a", "b"], ["a", "b", "c"]]'
OK
127.0.0.1:6379> JSON.ARRLEN k1 [*]
(integer) 0
127.0.0.1:6379> JSON.ARRLEN k1 $[3]
1) (integer) 3

127.0.0.1:6379> JSON.SET k2 . '[[], "a", ["a", "b"], ["a", "b", "c"], 4]'
OK
127.0.0.1:6379> JSON.ARRLEN k2 [*]
(integer) 0
127.0.0.1:6379> JSON.ARRLEN k2 $[1]
1) (nil)
127.0.0.1:6379> JSON.ARRLEN k2 $[2]
1) (integer) 2
```