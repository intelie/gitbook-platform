# Queries

INTELIE Live performs queries, possibly using different providers, to analyze data. The default query provider is [INTELIE Pipes](https://pipes.intelie.com), although queries can be executed in other systems and sub-systems.

Queries are performed in two phases: First, they are run in all storage providers available, respecting the time period defined in `span` parameter. When the historical query ends, then a real time query starts to wait for new data.

{% hint style="info" %}
A query can be started and stopped many times on its life cycle for different reasons, such as changes on dependencies or arbitrary actions. It cannot be started again only after it is destroyed.
{% endhint %}

## Life cycle

A query outputs different events during its life cycle. Those events can be of one of the types below.

| Type                                                                                                                                                                     |   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | - |
| `start`                                                                                                                                                                  |   |
| This event is emitted every time that a query starts.                                                                                                                    |   |
| `endHistory`                                                                                                                                                             |   |
| This event is emitted when the storage queries end.                                                                                                                      |   |
| `stop`                                                                                                                                                                   |   |
| This event is emitted every time that a query stops. A new `start` can be emitted after this event.                                                                      |   |
| `destroy`                                                                                                                                                                |   |
| This event is emitted once, when the query is destroyed. No further event is expected after this one.                                                                    |   |
| `progress`                                                                                                                                                               |   |
| This event contains information about the progress of queries on storage providers.                                                                                      |   |
| `span`                                                                                                                                                                   |   |
| This event is emitted once a minute with information about the current `span`. For example, a span `last hour` will have different start and end on every minute.        |   |
| `warning`                                                                                                                                                                |   |
| This event contains warnings about the query execution.                                                                                                                  |   |
| `performance`                                                                                                                                                            |   |
| This event contains information about the query performance.                                                                                                             |   |
| `skippedRealtime`                                                                                                                                                        |   |
| This event is emitted when any event would be received by this query if it didn't have the `__skiprealtime` modifier ([more](../pipes-queries/event-flow-modifiers.md)). |   |
| `startRealTime`                                                                                                                                                          |   |
| Similar to `endHistory`, occurs just before the real time query starts.                                                                                                  |   |
| `control`                                                                                                                                                                |   |
| This event can contain a set of control attributes.                                                                                                                      |   |
