# Redis JSON data type overview<a name="json-document-overview"></a>

ElastiCache for Redis supports a number of Redis commands for working with the JSON data type\. The following is an overview of the JSON data type and a detailed list of Redis commands that are supported\.

## Terminology<a name="json-terminology"></a>


****  

| Term | Description | 
| --- | --- | 
|  JSON document | Refers to the value of a Redis JSON key\. | 
|  JSON value | Refers to a subset of a JSON document, including the root that represents the entire document\. A value could be a container or an entry within a container\. | 
|  JSON element | Equivalent to JSON value\. | 

## Supported JSON standard<a name="Supported-JSON-Standard"></a>

JSON format is compliant with [RFC 7159](https://www.ietf.org/rfc/rfc7159.txt) and [ECMA\-404](https://www.ietf.org/rfc/rfc7159.txt) JSON data interchange standard\. UTF\-8 [Unicode](https://www.unicode.org/standard/WhatIsUnicode.html) in JSON text is supported\.

## Root element<a name="json-root-element"></a>

The root element can be of any JSON data type\. Note that in earlier RFC 4627, only objects or arrays were allowed as root values\. Since the update to RFC 7159, the root of a JSON document can be of any JSON data type\.

## Document size limit<a name="json-document-size-limit"></a>

JSON documents are stored internally in a format that's optimized for rapid access and modification\. This format typically results in consuming somewhat more memory than the equivalent serialized representation of the same document\. 

The consumption of memory by a single JSON document is limited to 64 MB, which is the size of the in\-memory data structure, not the JSON string\. You can check the amount of memory consumed by a JSON document by using the `JSON.DEBUG MEMORY` command\.

## JSON ACLs<a name="json-acls"></a>
+ Similar to the existing per\-datatype categories \(@string, @hash, etc\.\), a new category @json is added to simplify managing access to JSON commands and data\. No other existing Redis commands are members of the @json category\. All JSON commands enforce any keyspace or command restrictions and permissions\.
+ There are five existing Redis ACL categories that are updated to include the new JSON commands: @read, @write, @fast, @slow and @admin\. The following table indicates the mapping of JSON commands to the appropriate categories\.


**ACL**  

| JSON command | @read | @write | @fast | @slow | @admin | 
| --- | --- | --- | --- | --- | --- | 
|  JSON\.ARRAPPEND |  | y | y |  |  | 
|  JSON\.ARRINDEX | y |  | y |  |  | 
|  JSON\.ARRINSERT |  | y | y |  |  | 
|  JSON\.ARRLEN | y |  | y |  |  | 
|  JSON\.ARRPOP |  | y | y |  |  | 
|  JSON\.ARRTRIM |  | y | y |  |  | 
|  JSON\.CLEAR |  | y | y |  |  | 
|  JSON\.DEBUG | y |  |  | y | y | 
|  JSON\.DEL |  | y | y |  |  | 
|  JSON\.FORGET |  | y | y |  |  | 
|  JSON\.GET | y |  | y |  |  | 
|  JSON\.MGET | y |  | y |  |  | 
|  JSON\.NUMINCRBY |  | y | y |  |  | 
|  JSON\.NUMMULTBY |  | y | y |  |  | 
|  JSON\.OBJKEYS | y |  | y |  |  | 
|  JSON\.OBJLEN | y |  | y |  |  | 
|  JSON\.RESP | y |  | y |  |  | 
|  JSON\.SET |  | y |  | y |  | 
|  JSON\.STRAPPEND |  | y | y |  |  | 
|  JSON\.STRLEN | y |  | y |  |  | 
|  JSON\.STRLEN | y |  | y |  |  | 
|  JSON\.TOGGLE |  | y | y |  |  | 
|  JSON\.TYPE | y |  | y |  |  | 
|  JSON\.NUMINCRBY |  | y | y |  |  | 

## Nesting depth limit<a name="json-nesting-depth-limit"></a>

When a JSON object or array has an element that is itself another JSON object or array, that inner object or array is said to “nest” within the outer object or array\. The maximum nesting depth limit is 128\. Any attempt to create a document that contains a nesting depth greater than 128 will be rejected with an error\.

## Command syntax<a name="json-command-syntax"></a>

Most commands require a Redis key name as the first argument\. Some commands also have a path argument\. The path argument defaults to the root if it's optional and not provided\.

 Notation:
+ Required arguments are enclosed in angle brackets\. For example: <key>
+ Optional arguments are enclosed in square brackets\. For example: \[path\]
+ Additional optional arguments are indicated by an ellipsis \("…"\)\. For example: \[json \.\.\.\]

## Path syntax<a name="json-path-syntax"></a>

Redis JSON supports two kinds of path syntaxes:
+ **Enhanced syntax** – Follows the JSONPath syntax described by [Goessner](https://goessner.net/articles/JsonPath/), as shown in the following table\. We've reordered and modified the descriptions in the table for clarity\.
+ **Restricted syntax** – Has limited query capabilities\.

**Note**  
Results of some commands are sensitive to which type of path syntax is used\.

 If a query path starts with '$', it uses the enhanced syntax\. Otherwise, the restricted syntax is used\.

**Enhanced syntax**


****  

| Symbol/Expression | Description | 
| --- | --- | 
|  $ | The root element\. | 
|  \. or \[\] | Child operator\. | 
|  \.\. | Recursive descent\. | 
|  \* | Wildcard\. All elements in an object or array\. | 
|  \[\] | Array subscript operator\. Index is 0\-based\. | 
|  \[,\] | Union operator\. | 
|  \[start:end:step\] | Array slice operator\. | 
|  ?\(\) | Applies a filter \(script\) expression to the current array or object\. | 
|  \(\) | Filter expression\. | 
|  @ | Used in filter expressions that refer to the current node being processed\. | 
|  == | Equal to, used in filter expressions\. | 
|  \!= | Not equal to, used in filter expressions\. | 
|  > | Greater than, used in filter expressions\. | 
|  >= | Greater than or equal to, used in filter expressions\.  | 
|  < | Less than, used in filter expressions\. | 
|  <= | Less than or equal to, used in filter expressions\.  | 
|  && | Logical AND, used to combine multiple filter expressions\. | 
|  \|\| | Logical OR, used to combine multiple filter expressions\. | 

**Examples**

The following examples are built on [Goessner's](https://goessner.net/articles/JsonPath/) example XML data, which we have modified by adding additional fields\.

```
{ "store": {
    "book": [ 
      { "category": "reference",
        "author": "Nigel Rees",
        "title": "Sayings of the Century",
        "price": 8.95,
        "in-stock": true,
        "sold": true
      },
      { "category": "fiction",
        "author": "Evelyn Waugh",
        "title": "Sword of Honour",
        "price": 12.99,
        "in-stock": false,
        "sold": true
      },
      { "category": "fiction",
        "author": "Herman Melville",
        "title": "Moby Dick",
        "isbn": "0-553-21311-3",
        "price": 8.99,
        "in-stock": true,
        "sold": false
      },
      { "category": "fiction",
        "author": "J. R. R. Tolkien",
        "title": "The Lord of the Rings",
        "isbn": "0-395-19395-8",
        "price": 22.99,
        "in-stock": false,
        "sold": false
      }
    ],
    "bicycle": {
      "color": "red",
      "price": 19.95,
      "in-stock": true,
      "sold": false
    }
  }
}
```


****  

| Path | Description | 
| --- | --- | 
|  $\.store\.book\[\*\]\.author | The authors of all books in the store\. | 
|  $\.\.author | All authors\. | 
|  $\.store\.\* | All members of the store\. | 
|  $\["store"\]\.\* | All members of the store\. | 
|  $\.store\.\.price | The price of everything in the store\. | 
|  $\.\.\* | All recursive members of the JSON structure\. | 
|  $\.\.book\[\*\] | All books\. | 
|  $\.\.book\[0\] | The first book\. | 
|  $\.\.book\[\-1\] | The last book\. | 
|  $\.\.book\[0:2\] | The first two books\. | 
|  $\.\.book\[0,1\] | The first two books\. | 
|  $\.\.book\[0:4\] | Books from index 0 to 3 \(ending index is not inclusive\)\. | 
|  $\.\.book\[0:4:2\] | Books at index 0, 2\. | 
|  $\.\.book\[?\(@\.isbn\)\] | All books with an ISBN number\. | 
|  $\.\.book\[?\(@\.price<10\)\] | All books cheaper than $10\. | 
|  '$\.\.book\[?\(@\.price < 10\)\]' | All books cheaper than $10\. \(The path must be quoted if it contains white spaces\.\) | 
|  '$\.\.book\[?\(@\["price"\] < 10\)\]' | All books cheaper than $10\. | 
|  '$\.\.book\[?\(@\.\["price"\] < 10\)\]' | All books cheaper than $10\. | 
|  $\.\.book\[?\(@\.price>=10&&@\.price<=100\)\] | All books in the price range of $10 to $100, inclusive\. | 
|  '$\.\.book\[?\(@\.price>=10 && @\.price<=100\)\]' | All books in the price range of $10 to $100, inclusive\. \(The path must be quoted if it contains white spaces\.\) | 
|  $\.\.book\[?\(@\.sold==true\|\|@\.in\-stock==false\)\] | All books sold or out of stock\. | 
|  '$\.\.book\[?\(@\.sold == true \|\| @\.in\-stock == false\)\]' | All books sold or out of stock\. \(The path must be quoted if it contains white spaces\.\) | 
|  '$\.store\.book\[?\(@\.\["category"\] == "fiction"\)\]' | All books in the fiction category\. | 
|  '$\.store\.book\[?\(@\.\["category"\] \!= "fiction"\)\]' | All books in nonfiction categories\. | 

Additional filter expression examples:

```
127.0.0.1:6379> JSON.SET k1 . '{"books": [{"price":5,"sold":true,"in-stock":true,"title":"foo"}, {"price":15,"sold":false,"title":"abc"}]}'
OK
127.0.0.1:6379> JSON.GET k1 $.books[?(@.price>1&&@.price<20&&@.in-stock)]
"[{\"price\":5,\"sold\":true,\"in-stock\":true,\"title\":\"foo\"}]"
127.0.0.1:6379> JSON.GET k1 '$.books[?(@.price>1 && @.price<20 && @.in-stock)]'
"[{\"price\":5,\"sold\":true,\"in-stock\":true,\"title\":\"foo\"}]"
127.0.0.1:6379> JSON.GET k1 '$.books[?((@.price>1 && @.price<20) && (@.sold==false))]'
"[{\"price\":15,\"sold\":false,\"title\":\"abc\"}]"
127.0.0.1:6379> JSON.GET k1 '$.books[?(@.title == "abc")]'
[{"price":15,"sold":false,"title":"abc"}]

127.0.0.1:6379> JSON.SET k2 . '[1,2,3,4,5]'
127.0.0.1:6379> JSON.GET k2 $.*.[?(@>2)]
"[3,4,5]"
127.0.0.1:6379> JSON.GET k2 '$.*.[?(@ > 2)]'
"[3,4,5]"

127.0.0.1:6379> JSON.SET k3 . '[true,false,true,false,null,1,2,3,4]'
OK
127.0.0.1:6379> JSON.GET k3 $.*.[?(@==true)]
"[true,true]"
127.0.0.1:6379> JSON.GET k3 '$.*.[?(@ == true)]'
"[true,true]"
127.0.0.1:6379> JSON.GET k3 $.*.[?(@>1)]
"[2,3,4]"
127.0.0.1:6379> JSON.GET k3 '$.*.[?(@ > 1)]'
"[2,3,4]"
```

**Restricted syntax**


****  

| Symbol/Expression | Description | 
| --- | --- | 
|  \. or \[\] | Child operator\. | 
|  \[\] | Array subscript operator\. Index is 0\-based\. | 

**Examples**


****  

| Path | Description | 
| --- | --- | 
|  \.store\.book\[0\]\.author | The author of the first book\. | 
|  \.store\.book\[\-1\]\.author | The author of the last book\. | 
|  \.address\.city | City name\. | 
|  \["store"\]\["book"\]\[0\]\["title"\] | The title of the first book\. | 
|  \["store"\]\["book"\]\[\-1\]\["title"\] | The title of the last book\. | 

**Note**  
All [Goessner](https://goessner.net/articles/JsonPath/) content cited in this documentation is subject to the [Creative Commons License](https://creativecommons.org/licenses/by/2.5/)\.

## Common error prefixes<a name="json-error-prefixes"></a>

Each error message has a prefix\. The following is a list of common error prefixes\.


****  

| Prefix | Description | 
| --- | --- | 
|  ERR | A general error\. | 
|  LIMIT | An error that occurs when the size limit is exceeded\. For example, the document size limit or nesting depth limit was exceeded\. | 
|  NONEXISTENT | A key or path does not exist\. | 
|  OUTOFBOUNDARIES | Array index out of bounds\. | 
|  SYNTAXERR | Syntax error\. | 
|  WRONGTYPE | Wrong value type\. | 

## JSON\-related metrics<a name="json-info-metrics"></a>

The following JSON info metrics are provided:


****  

| Info | Description | 
| --- | --- | 
|  json\_total\_memory\_bytes | Total memory allocated to JSON objects\. | 
|  json\_num\_documents | Total number of documents in Redis\. | 

To query core metrics, run the following Redis command:

```
info json_core_metrics
```

## How ElastiCache for Redis interacts with JSON<a name="json-differences"></a>

The following section describes how ElastiCache for Redis interacts with the JSON data type\.

### Operator precedence<a name="json-operator-precedence"></a>

When evaluating conditional expressions for filtering, &&s take precedence first, and then \|\|s are evaluated, as is common across most languages\. Operations inside of parentheses are run first\. 

### Maximum path nesting limit behavior<a name="json-max-path"></a>

 The maximum path nesting limit in ElastiCache for Redis is 128\. So a value like `$.a.b.c.d...` can only reach 128 levels\. 

### Handling numeric values<a name="json-about-numbers"></a>

JSON doesn't have separate data types for integers and floating point numbers\. They are all called numbers\.

Numerical representations:

When a JSON number is received on input, it is converted into one of the two internal binary representations: a 64\-bit signed integer or a 64\-bit IEEE double precision floating point\. The original string and all of its formatting are not retained\. Thus, when a number is output as part of a JSON response, it is converted from the internal binary representation to a printable string that uses generic formatting rules\. These rules might result in a different string being generated than was received\.

Arithmetic commands `NUMINCRBY` and `NUMMULTBY`:
+ If both numbers are integers and the result is out of the range of `int64`, it automatically becomes a 64\-bit IEEE double precision floating point number\.
+ If at least one of the numbers is a floating point, the result is a 64\-bit IEEE double precision floating point number\.
+ If the result exceeds the range of 64\-bit IEEE double, the command returns an `OVERFLOW` error\.

For a detailed list of available commands, see [Supported Redis JSON commands](json-list-commands.md)\.

### Direct array filtering<a name="json-direct-array-filtering"></a>

ElastiCache for Redis filters array objects directly\.

For data like `[0,1,2,3,4,5,6]` and a path query like `$[?(@<4)]`, or data like `{"my_key":[0,1,2,3,4,5,6]}` and a path query like `$.my_key[?(@<4)]`, ElastiCache for Redis would return \[1,2,3\] in both circumstances\. 

### Array indexing behavior<a name="json-direct-array-indexing"></a>

ElastiCache for Redis allows both positive and negative indexes for arrays\. For an array of length five, 0 would query the first element, 1 the second, and so on\. Negative numbers start at the end of the array, so \-1 would query the fifth element, \-2 the fourth element, and so on\.

To ensure predictable behavior for customers, ElastiCache for Redis does not round array indexes down or up, so if you have an array with a length of 5, calling index 5 or higher, or \-6 or lower, would not produce a result\.

### Strict syntax evaluation<a name="json-strict-syntax-evaluation"></a>

MemoryDB does not allow JSON paths with invalid syntax, even if a subset of the path contains a valid path\. This is to maintain correct behavior for our customers\.