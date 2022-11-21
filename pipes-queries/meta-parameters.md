# Meta parameters

Intelie Live supports a set of meta parameters that can change the behavior of any Pipes query.

All the meta parameters listed below can be used in three different ways:

* `#property=value` on the span of a query
* `.property:value` on the filter of a query
* `=> @meta value as property` on the body of a query

{% hint style="info" %}
Some meta parameters can also change the behavior of some [frontend features](../data-visualization/pipes-modifiers-on-standard-charts.md). Those parameters can only be used with the `@meta` pipe.
{% endhint %}

| Parameter                                                                                                                                                     | Type               | Since   |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------ | ------- |
| `storage`                                                                                                                                                     | boolean            |         |
| Forces a real time query to run entirely from the storage.                                                                                                    |                    |         |
| `future` or `allowFuture`                                                                                                                                     | boolean            |         |
| Allows spans that reference events in future. This implies the query will run entirely on storage (using simulated real time, when necessary).                |                    |         |
| `disableLatestOptimization`                                                                                                                                   | boolean            |         |
| Disable span optimization for `@latest` reducers.                                                                                                             |                    |         |
| `disableRealTime`                                                                                                                                             | boolean            |         |
| Override query's realtime spec.                                                                                                                               |                    |         |
| `forceRealTime`                                                                                                                                               | boolean            |         |
| Force query to stay alive after history load even if not required. This attribute has precedence over `disableRealtime`.                                      |                    |         |
| `disablePreload`                                                                                                                                              | boolean            |         |
| Override query's preload spec.                                                                                                                                |                    |         |
| `truncate`                                                                                                                                                    | boolean            |         |
| Valid only on datasource reset: it determines whether the reset procedure. It should truncate the whole collection before starting the procedure.             |                    |         |
| `lazy`                                                                                                                                                        | boolean            |         |
| Lazily advance query clock using events only (no tick).                                                                                                       |                    |         |
| `disableQueryStats`                                                                                                                                           | boolean            |         |
| Disable collection of query stats.                                                                                                                            |                    |         |
| `extendhistory`                                                                                                                                               | string             |         |
| Asks Live for more data to the left, without really expanding the span.                                                                                       |                    |         |
| `extendfuture`                                                                                                                                                | string             |         |
| Asks Live for more data to the right, without really expanding the span.                                                                                      |                    |         |
| `partial`                                                                                                                                                     | integer            |         |
| <p>Asks Live for only a few of the events (it can return a bit more).</p><p>Example in a span: <br><code>since ts 0 #partial='12'</code></p>                  |                    |         |
| `downloadFile`                                                                                                                                                | string             |         |
| Defines the downloaded filename (extension will be added)                                                                                                     |                    |         |
| `warningThreshold`                                                                                                                                            | string             |         |
| Defines the minimum severity for a warning to be emitted                                                                                                      |                    |         |
| `pipes.group`                                                                                                                                                 | string, seq or map | 2.25.12 |
| Replaces the automatic `group` definition with a constant list of properties. This parameters only works with `@meta` and only for the `pipes`query provider. |                    |         |
