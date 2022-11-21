# live.properties

This file contains the basic configuration for Intelie Live runtime, and is located at `/opt/intelie/live/conf`.

| Property                                                                                                                                                                                                                                                                          | Type   | Default             | Since  |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------ | ------------------- | ------ |
| `datasource.url`                                                                                                                                                                                                                                                                  | string |                     |        |
| `datasource.user`                                                                                                                                                                                                                                                                 | string |                     |        |
| `datasource.password`                                                                                                                                                                                                                                                             | string |                     |        |
| `datasource.doNotMigrate`                                                                                                                                                                                                                                                         | string | false               |        |
| If true, Live will not try to run the migrations to update the database to the latest version. In this case, the migrations must be performed externally.                                                                                                                         |        |                     |        |
| `datasource.maxIdleTime`                                                                                                                                                                                                                                                          | int    | 1800                |        |
| `datasource.maxPoolSize`                                                                                                                                                                                                                                                          | int    | 50                  |        |
| `datasource.minPoolSize`                                                                                                                                                                                                                                                          | int    | 2                   |        |
| `datasource.maxStatements`                                                                                                                                                                                                                                                        | int    | 0                   |        |
| `datasource.maxStatementsPerConnection`                                                                                                                                                                                                                                           | int    | 0                   |        |
| `datasource.initialPoolSize`                                                                                                                                                                                                                                                      | int    | 2                   |        |
| `datasource.acquireIncrement`                                                                                                                                                                                                                                                     | int    | 1                   |        |
| `datasource.checkoutTimeout`                                                                                                                                                                                                                                                      | int    | 3000                |        |
| `datasource.idleConnectionTestPeriod`                                                                                                                                                                                                                                             | int    | 30                  |        |
| `datasource.preferredTestQuery`                                                                                                                                                                                                                                                   | string | SELECT 1            |        |
| `live.home`                                                                                                                                                                                                                                                                       |        | /var/lib/live       |        |
| Root directory, where to look for the plugins (`/plugins`), drivers (`/drivers`), application data (`/data`), cache (`/cache`) and persistent queues (`/queues`)                                                                                                                  |        |                     |        |
| `live.cache_home`                                                                                                                                                                                                                                                                 | string | \[live.home]/cache  | 2.29.0 |
| `live.queues_home`                                                                                                                                                                                                                                                                | string | \[live.home]/queues | 2.29.0 |
| `live.warn.threshold.flow`                                                                                                                                                                                                                                                        | int    | 100                 | 3.1.0  |
| An warn will be emitted when flow lasts longer than the configured value, in milliseconds.                                                                                                                                                                                        |        |                     |        |
| `live.warn.threshold.tick`                                                                                                                                                                                                                                                        | int    | 10                  | 3.1.0  |
| An warn will be emitted when tick lasts longer than the configured value, in milliseconds.                                                                                                                                                                                        |        |                     |        |
| `live.warn.period.cooldown`                                                                                                                                                                                                                                                       | int    | 1000                | 3.1.0  |
| Warns that occur too often, within this cooldown period, will be omitted.                                                                                                                                                                                                         |        |                     |        |
| `security.passwordEncryptionPassword`                                                                                                                                                                                                                                             | string |                     | 3.0.0  |
| Live will encrypt password of extensions and will update the database with encrypted passwords. Should be a unique strong and secure key string. If changed, all passwords of extensions will be lost and need to be entered again.                                               |        |                     |        |
| `settings.readcache`                                                                                                                                                                                                                                                              | int    | 67108864 (64MB)     |        |
| <p>How much data will be cached from the settings for reading. This value is in bytes. Each settings' value and path cached will consume this limit.  Be aware that the memory consumption for this cache will be greater than this value.</p><p></p><p>This is an LRU cache.</p> |        |                     |        |
| `settings.writebuffer`                                                                                                                                                                                                                                                            | int    | 1048576 (1MB)       |        |
| <p>How much data will be buffered in settings' write operation. This value is in bytes. Each settings' value and path cached will consume this limit. </p><p>If the limit is reached the buffer is flushed.</p>                                                                   |        |                     |        |
| `settings.flushperiod`                                                                                                                                                                                                                                                            | int    | 5000 (5 seconds)    |        |
| Interval in milliseconds that the settings' buffer will be automatically flushed, i.e. will be persisted.                                                                                                                                                                         |        |                     |        |

## Example

```
# Intelie Live configuration properties

#Any property value can refer to an environment variable, using the syntax:
#
#Example:
#datasource.user=${USER}_mysql

#DB
datasource.url=jdbc:mysql://localhost/live?autoReconnect=true&characterEncoding=utf-8
datasource.user=live
datasource.password=
datasource.doNotMigrate=false

#Directory where plugins and jdbc-drivers are installed

#optional
#live.home=runtime
#live.cache_home=/tmp/myCache
#live.queues_home=/tmp/myQueues
```
