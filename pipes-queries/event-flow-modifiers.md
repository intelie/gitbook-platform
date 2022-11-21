# Event flow modifiers

Some fields defined on an event can change the data flow in Intelie Live.

| Field                                                                                                                                                                                                                                                                                                                        | Type                    |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- |
| `__overwrite`                                                                                                                                                                                                                                                                                                                | string or list\<string> |
| A field or list of fields that will be used as indexes to update previous events. If multiple events match the criteria, one will be selected arbitrarily. If no event matches the criteria, a new one is created. When using postgres storage, if N events match, they will all be removed and only new event will be kept  |                         |
| `__skiprealtime`                                                                                                                                                                                                                                                                                                             | boolean                 |
| If true, the event is persisted on the storage but is not delivered to the real time engine.                                                                                                                                                                                                                                 |                         |
| `__skipstorage`                                                                                                                                                                                                                                                                                                              | boolean                 |
| If true, the event is delivered to the real time engine but is not persisted on the storage.                                                                                                                                                                                                                                 |                         |

## Examples

In the example below, the first event will be overwritten by the second one.

```bash
echo '{"__type":"some_type", key: k, a: 1}' | nc localhost 17042
```

```bash
echo '{"__type":"some_type", key: k, a: 2, __overwrite:key}' | nc localhost 17042
```

