# JSON\.CLEAR<a name="json-clear"></a>

Clears the arrays or an object at the path\.

Syntax

```
JSON.CLEAR <key> [path]
```
+ key \(required\) – A Redis key of JSON document type\.
+ path \(optional\) – A JSON path\. Defaults to the root if not provided\.

**Return**
+ Integer, the number of containers cleared\.
+ Clearing an empty array or object accounts for 1 container cleared\.
+ Clearing a non\-container value returns 0\.

**Examples**

```
127.0.0.1:6379> JSON.SET k1 . '[[], [0], [0,1], [0,1,2], 1, true, null, "d"]'
OK
127.0.0.1:6379>  JSON.CLEAR k1  $[*]
(integer) 7
127.0.0.1:6379> JSON.CLEAR k1  $[*]
(integer) 4
127.0.0.1:6379> JSON.SET k2 . '{"children": ["John", "Jack", "Tom", "Bob", "Mike"]}'
OK
127.0.0.1:6379> JSON.CLEAR k2 .children
(integer) 1
127.0.0.1:6379> JSON.GET k2 .children
"[]"
```