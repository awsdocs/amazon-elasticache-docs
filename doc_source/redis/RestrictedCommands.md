# Restricted Redis commands<a name="RestrictedCommands"></a>

To deliver a managed service experience, Redis restricts access to certain cache engine\-specific commands that require advanced privileges\. For cache clusters running Redis, the following commands are unavailable:
+ `acl setuser`
+ `acl load`
+ `acl save`
+ `acl deluser`
+ `bgrewriteaof`
+ `bgsave`
+ `cluster addslot`
+ `cluster addslotsrange`
+ `cluster bumpepoch`
+ `cluster delslot`
+ `cluster delslotsrange `
+ `cluster failover `
+ `cluster flushslots `
+ `cluster forget `
+ `cluster links`
+ `cluster meet`
+ `cluster setslot`
+ `config`
+ `debug`
+ `migrate`
+ `psync`
+ `replicaof`
+ `save`
+ `slaveof`
+ `shutdown`
+ `sync`