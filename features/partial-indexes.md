---
description: >-
  Optionally, Live enables you to create partial indexes for event types that
  require special attention regarding the space consumed by indexes or query
  performance for small slices of data.
---

# Partial Indexes

Live enables the plugins to register indexes for some specific event types through the Live API. Partial indexes are just an option the developer can use to specify a Pipes term filter expression to reduce the index size and enhance the efficiency of storage queries in a storage provider.

{% hint style="info" %}
Partial indexes API is featured at Live 3.0.0 and depends on the underlying storage provider.
{% endhint %}

## Registration

```java
import net.intelie.live.Event;
import net.intelie.live.IndexDef;
import net.intelie.live.Live;
import net.intelie.pipes.filters.Filter;
//...

Filter filter = live.pipes().compiler().filter().compile("field2:value");
live.index().registerIndex(new IndexDef("mydata", "field1").withPartial(filter));
```

The `filter` must be a Pipes term filter expression and should be compiled as demonstrated above. See more details about Pipes term filters on: [https://pipes.intelie.com/docs/0.24/#chapter-filters](https://pipes.intelie.com/docs/0.24/#chapter-filters)

The expression exemplified above will create the index for `field1` only in case of `field2` assume the string `value`. The support for type cast and other operators is provided from the Pipes perspective.&#x20;

Those expressions written in Pipes language will be translated to the specific Storage Provider query language, thus each storage provider may limit the operators supported.

## Limitations

Currently **Plugin MongoDB** and **Plugin PostgreSQL** support partial indexes. Thus, if you try to use partial indexes in Live Index subsystem combined to any other storage provider, you may check the capabilities of storage provider implementation being used.

### MongoDB

Starting at 3.0.0, the Live plugin for MongoDB supports only a few partial index expressions since it relies on native MongoDB partial index expressions. MongoDB partial index expressions supports a few operators as described in MongoDB official site: [https://docs.mongodb.com/manual/core/index-partial](https://docs.mongodb.com/manual/core/index-partial/).

For short, partial indexes in MongoDB installations will support only:

* equality expressions (i.e. `field:value`)
* range operators (i.e. `field:[200, 300)`)
* and operators (i.e. `field1:value1 field2:value2`)

### PostgreSQL

Starting at 3.2.0, the Live plugin for PostgreSQL supports partial indexes by converting the Pipes filter expressions to `WHERE`clauses in PostgreSQL. In [Pipes filter syntax](https://pipes.intelie.com/docs/0.24/#quickstart-filters) it's easy to write most of range, literal comparison and wildcards operators that will be easily mapped into SQL statements.

For further details about the native support for partial indexes at PosgreSQL, see [https://www.postgresql.org/docs/12/indexes-partial.html](https://www.postgresql.org/docs/12/indexes-partial.html).
