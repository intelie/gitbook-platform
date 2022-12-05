---
description: Enables the data crawling from ordinary SQL databases as events
---

# SQL

The SQL plugin is available for download on [Intelie Live Marketplace](https://marketplace.intelie.com/artifact/plugin-sql) and is composed by two main components which implements **Query Provider** and **Storage Provider** features. Those features can be enabled by selecting the roles when creating a new instance in Integrations menu at administration page.

![Roles provided by Live SQL Plugin](<../.gitbook/assets/image (79).png>)

## Storage Provider

Once the Storage role is selected, a new section named **SQL Events configuration** will be available at configuration screen.

![SQL events configuration form](<../.gitbook/assets/image (111).png>)

The basic idea of this section is to define some event types that when queried using Pipes (in history mode using a time span) the data will be fetched from database using a configured SQL Template.

A SQL Event configuration is composed by:

* Pipes event name (the event type that will be used in Pipes query)
* Query template (the query definition with expansion of placeholders)
* Available Fields (fields that will be returned by the query)

### Query template

A template for building the SQL query that fetches the events. This is an example of a minimal query template in MySQL:

```
select 
    {fields} 
from 
    mytable 
where 
    {timestamp} >= FROM_UNIXTIME({start}/1000) 
    and {timestamp} < FROM_UNIXTIME({end}/1000) 

{ifWhere} and {/ifWhere} 
    {where}

{orderBy}ORDER_FIELD_1; ORDER_FIELD_2{/orderBy}
    
{ifLimit}limit {limit}{/ifLimit}
```

The markers inside curly braces will be expanded at runtime and each one has the following meaning:

* `{fields}` These are the fields we want to return. If no `.select` hint was passed all fields defined in available fields table will be returned, otherwise just the fields defined inside the hint.
* `{timestamp}` will be replaced by timestamp field expression.
* `start` and `end` will be replaced with start and end of the queried time span.
* `{ifWhere}...{/ifWhere}` This will be included only if additional filters are passed through **.where** storage hint.
* `{orderBy}ORDER_FIELD_1; ORDER_FIELD_2{/orderBy}` will be replaced by `order by ORDER_FIEL_1 asc, ORDER_FIELD_2 asc`. In case **.flags:reversed** be used it will expand to the descending order statement \``order by ORDER_FIEL_1 desc, ORDER_FIELD_2 desc`. To keep consistency with Live event model it is recommended to use the timestamp column as the first order by field.
* `{ifLimit} inner limit expression {limit} {/ifLimit}` if a **.limit** storage hint was informed this will be replaced by inner limit expression with limit value provided by the hint. The inner limit expression is database specific e.g `limit {limit}` for MySQL, `top {limit}` for MSSQL (position also changes in this case), `rownum < {limit}` for Oracle.

#### Timestamp and date time formats

By default **start** and **end** markers are expanded as the default Pipes timestamp - milliseconds since the Epoch, similar to the Unix time but with millisecond resolution. The template is responsible for applying the necessary conversion functions to compare with the actual timestamp fields.

If your database does not support conversion from this timestamp format, alternative expansion options can be configured in the tag: `{start:yyyyMMdd_HHmmss}` expands as a string in the given format. `{start:yyyyMMdd_HHmmss@America/Sao_Paulo}` or `{end:yyyy-MM-dd'T'HH:mm:ss@GMT-5}` also include a timezone for the conversion.

### Available fields

![Table mapping pipes properties to SQL expressions](<../.gitbook/assets/image (115).png>)

The table mapping a pipes property to a respective database expression. A table expression can be:

* A column e.g: a pipes property `name` maps to a database column called `name`.
* The result of database function e.g: `TRIM(name)`
* A column with a table identification `table1.name`
* An aggregation of various database columns in different tables `CONCAT(table1.name, ' - ', table2.age)`

The SQL mappings are used when the SQL template is expanded. For example:

* `{fields}` marker will expand each SQL expression in field mapping as follows
  * `name as name`
  * `logs.event->>'$.sensorType' as sensorType`
  * `TRIM(name) as trimmedName`
  * `CONCAT(table1.name, ' - ', table2.age) as nameAge`
  * `joinTable.columnX as columnX`
* `{where}` marker will expand whenever `.where` storage hint is used as follows
  * Pipes: `.where:'sensorType$:temperature'`&#x20;
  * SQL field mapping: `sensorType = logs.event ->> '\$.sensorType'`&#x20;
  * Where clause expression expanded: `logs.event ->> '$.sensorType' = 'temperature'`

Starting at SQL Plugin 3.1.0, JSON type columns are supported.

**Finally, it is mandatory to configure at least the `timestamp` field.**

## Query Provider

Live API enables the creation of customized query providers. Live SQL Plugin delivers a new option at Live Console to execute SQL queries over the SQL integrations currently configured.

![Query providers are available at console screen](<../.gitbook/assets/image (90).png>)
