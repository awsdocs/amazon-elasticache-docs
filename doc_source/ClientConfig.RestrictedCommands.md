# Restricted Commands<a name="ClientConfig.RestrictedCommands"></a>

In order to deliver a managed service experience, ElastiCache restricts access to certain cache engine\-specific commands that require advanced privileges\.

+ For cache clusters running Memcached, there are no restricted commands\.

+ For cache clusters running Redis, the following commands are unavailable:

  + `bgrewriteaof`

  + `bgsave`

  + `config`

  + `debug`

  + `migrate`

  + `save`

  + `slaveof`

  + `shutdown`

  + `sync`