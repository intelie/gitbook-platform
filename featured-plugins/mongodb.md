---
description: Enables the data crawling from MongoDB document stores as events
---

# MongoDB

## Storage Provider

INTELIE Live delivers the capability of storing events as documents in MongoDB servers. The MongoDB Storage Provider is the most deployed plugin in our customer environments.

The event type for Live will become the collection name for the documents. The timestamp field will be used as the default ordering criteria.

This plugin supports the management of indexes and collections by its own. Custom indexes can be created directly from the Integrations administration page.

![Collections and indexes management capabitilities provided at administration page for a MongoDB integration instance](../.gitbook/assets/Screenshot\_select-area\_20211215004046.png)

Live index semantics is strongly inspired in the MongoDB index capabilities. See [partial indexes](../features/partial-indexes.md#mongodb) to understand advanced usages.

A storage provider is not used by users directly but will be used indirectly once you fired historical queries through Pipes expressions. Live MongoDB plugin also supports many [storage hints](../pipes-queries/storage-hints.md) in order to improve the query performance by change the ordering criteria, limit the results or specify conditions to find the documents.

Specially the `.where`storage hint can take advantage in performance once you have custom indexes to support more efficient query plans. Most of expressions written in Pipes will be translated automatically for MongoDB find criterias.

You can identify the queries being fired at MongoDB installations easily from the Integrations administration page for each storage provider you have configured.

![Queries tab and option to inquire the MongoDB server to explain the winning plan from the internal MongoDB query planner](../.gitbook/assets/Screenshot\_select-area\_20211215003805.png)

## Query Provider

Live API enables the creation of customized query providers. Live MongoDB Plugin delivers a new option at Live Console to execute MongoDB database commands over the MongoDB integrations currently configured.

![Dropdown at Live Console list all MongoDB integrations available to execute commands](<../.gitbook/assets/image (140).png>)

In most of installations, database commands can be executed but the MongoDB database administrator can limit the permissions for the database configured for the Live instance.&#x20;

Apart of that, the query provider can follow the query period definition, so in order to execute a single command with no periodic semantics you must start by typing `//@noeval` as a marker in the code area. After, in a new line, you can use any MongoDB [query commands](https://www.mongodb.com/docs/v4.2/reference/command/nav-crud/) or [database command](https://docs.mongodb.com/v4.2/tutorial/use-database-commands/).

For example, type the following, uncheck the **Follow query period definition** and click to **Run**.

```
//@noeval
{ buildInfo: 1 }
```

![Example of execution of the buildInfo database command using MongoDB Query Provider](<../.gitbook/assets/image (18).png>)
