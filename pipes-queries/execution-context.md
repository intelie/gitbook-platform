# Execution Context

Intelie Live provides a set of macros and functions representing the execution context.

| Name                                                                                    | Type       | Return type          |
| --------------------------------------------------------------------------------------- | ---------- | -------------------- |
| `@@online`                                                                              | `macro`    | `boolean`            |
| When used in a filter, becomes true after loading history. Otherwise it's always false. |            |                      |
| `@@span`                                                                                | `macro`    | `string`             |
| Effective span to use on windows (e.g. may replace 'yesterday' with 'today').           |            |                      |
| `@@fullspan`                                                                            | `macro`    | `string`             |
| Full effective span applied to query (e.g. will keep 'yesterday').                      |            |                      |
| `@@userspan`                                                                            | `macro`    | `string`             |
| Span that was chosen by user.                                                           |            |                      |
| `@@output`                                                                              | `macro`    | `period`             |
| Best output option to yield approximately 5 to 12 events.                               |            |                      |
| `@@params`                                                                              | `macro`    | `map(string,object)` |
| Query parameters                                                                        |            |                      |
| `@@context`                                                                             | `macro`    | `map(string,object)` |
| The query context (more details below)                                                  |            |                      |
| `live.eventsize`                                                                        | `function` | `number`             |
| Returns the size of the event                                                           |            |                      |
| `live.output`                                                                           | `function` | `period`             |
| Finds the best output period to yield points near a certain range                       |            |                      |
| `live.matchspan`                                                                        | `function` | `object`             |
| Finds the best mapped object that yield points near a certain range                     |            |                      |

{% hint style="info" %}
You can use Pipes span functions ([example](https://pipes.intelie.com/docs/#scalar-span)) to make calculations regarding `span` values.
{% endhint %}

## Query context (@@context)

| Field                                                                                                 |                      |
| ----------------------------------------------------------------------------------------------------- | -------------------- |
| `id`                                                                                                  | `number`             |
| Internal Live numeric identifier                                                                      |                      |
| `datasource`                                                                                          | `string`             |
| If the query is started by a datasource, the datasource identifier                                    |                      |
| `createdAt`                                                                                           | `timestamp`          |
| Time of query creation (first start)                                                                  |                      |
| `download`                                                                                            | `boolean`            |
| Whether this is a download query                                                                      |                      |
| `host`                                                                                                | `string`             |
| Hostname from where this query was started                                                            |                      |
| `url`                                                                                                 | `string`             |
| URL of the web element that requested the query creation                                              |                      |
| `description`                                                                                         | `string`             |
| Further details of that query                                                                         |                      |
| `user`                                                                                                | `map(string,object)` |
| Details of the user that requested the query creation, if any                                         |                      |
| `source`                                                                                              | `string`             |
| The object that originated the query, e.g. datasource, rule, etc.                                     |                      |
| `origin`                                                                                              | `string`             |
| The plugin or subsystem that requested the query creation                                             |                      |
| `timezone`                                                                                            | `string`             |
| Time zone of the user that requested the query creation                                               |                      |
| `stateId`                                                                                             | `string`             |
| An identifier that helps the query providers to identify a query and persist some state related to it |                      |

## Examples

```
live.eventsize([<object>])
```

```
live.output([<minPoints>], <maxPoints>)
```

```
live.matchspan([<min>], <max>, (<period>, <obj>)...)
```
