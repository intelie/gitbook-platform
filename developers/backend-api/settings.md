# Settings

The settings API provide a way to deal with any type of static data. Once saved in the settings, data can be used anywhere in the system, from Pipes queries to web interfaces, or even exported to other systems.

The settings are organized in nodes (`SettingsNode`) of a tree. Each node can have data associated to it. The root nodes represents plugins or the core.

```java
root = live.settings().root()
```

One can navigate on the settings using the `cd` method. For example:

```java
node = root.home().cd("user-config/%d", 42)
```

or

```java
node = root.home().cd("user-config").cd("42")
```

Both ways access the same node. Once in a node, one can save the data to it:

```java
node.set(obj);
```

And retrieve it:

```java
Object obj = node.get(Object.class);
```

The data associated to a node is serialized in JSON format and saved to the database, in the table `setting`. There is also a in-memory copy of the most accessed data.

## History logs

Every change to a node can generate a log, which will be saved as an entry on the table `setting_log`. This is optional (disabled by default) and can be enabled using the following:

To enable the log generation, the method `enableLog` is provided:

* When editing multiple nodes:

```java
node.withOptions(new SettingsOptions().enableLog(true), () -> {
    node.set(someValue);
    node.cd("child-54").set(anotherValue);
    node.cd("child-73").delete();
    return null;
});
```

* Live 3+ offers a simplified alternative per edition:

```java
node.set(someValue, new SettingsOptions().enableLog(true));
node.cd("child-61").delete(new SettingsOptions().enableLog(true));
```

Live 3+ offers this simplified version:

The setting logs can be used to provide a version history feature for new concepts or configurations.

```java
node.set(someValue, new SettingsOptions().enableLog(true));
```

## **Cache** Metrics



Live continuously and automatically collects metrics about the settings cache, those metrics can be accessed using pipes or through the system log.

All the metrics are related to the settings cache and can be accessed via query pipes.

In total there are eleven metrics that came from the settings cache and can be accessed using the following query:

`__metrics metric:settings.cache`    &#x20;

### Hit

The Hit metric reports the hit number in the settings cache, i.e., the number of times an accessed setting is present in the cache.

The higher this value, the greater the cache accuracy.\


### Miss

The Miss metric reports the number of cache misses, i.e., the number of times an accessed setting is not present in the cache.

The higher this value, the greater the number of writes in the cache. \


### Inserts

The Inserts metric reports the total number of inserts in the cache (by set + by miss), i.e., is a sum of all inserts made to the cache.

This metric is divided into two metrics: \


* Insert by set - This metric reports the number of inserts of new settings. Every time a new setting is created it is immediately added to the cache.&#x20;
* Insert by miss - This metric reports the number of inserts after misses. Every time there is a miss in the settings cache, the setting that generated this miss is inserted into the cache.&#x20;

### Overflow

The Overflow metric reports the number of cache overflows, i.e., whenever the total cache size is exceeded, an overflow occurs.

This metric can indicate whether the cache is correctly sized or not.\


### Removes

The Inserts metric reports the total number of removal in the cache (by delete + by overflow), i.e., is a sum of all cache removals.

This metric is divided into two metrics: \


* Remove by delete - This metric reports the number of deleted settings removals. Every time a setting is deleted from the system, if it was in the cache, it is also removed from the cache.&#x20;
* Remove by overflow - This metric reports the number of removals after an overflow. Every time a cache overflow occurs, keys that were least recently accessed are removed while the current cache size is greater than the maximum cache size.

### Max Weight

The Max Weight metric reports the cache max weight, i.e., reflects the maximum size of settings the cache will store.&#x20;

This metric is constant and serves as a reference for possible changes in cache size.\


### Current Weight

The Current Weight metric reports the cache current weight, i.e., reflects the cache size during the analyzed period.&#x20;

The higher this metric, the greater the number of settings persisted in the cache. If this value exceeds the maximum cache size, it will result in overflows. \
