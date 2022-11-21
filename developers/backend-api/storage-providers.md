# Storage Providers

A Storage Provider is an API that provides events to Live, giving the platform the means to connect to remote systems to achieve data persistence. It saves the incoming events, then gives the ability to read or delete them.

### Java API <a href="#user-content-basic-functionalities" id="user-content-basic-functionalities"></a>

```java
package net.intelie.live;

public interface EventTypesProvider {

    Set<String> allTypes();

}

public interface StorageProvider extends EventTypesProvider {

    void deleteAll(String type);

    void flow(Event event);

    EventIterator query(StorageQuery query, long start, long end, StorageQueryOptions options) throws Exception;

    long delete(StorageQuery query, long start, long end) throws Exception;

    StorageStatistics statistics();

    void ensureIndex(IndexDef index);

    IndexInfo checkIndex(IndexDef index);

    default void setLayout(@NotNull String type, @Nullable StorageLayout layout) {
    }

}
```

The basic functionalities are the **flow** (to save the events), **query** (to read the events), **delete** (to delete partially or totally the events), and **allTypes** (to list all known event types).

The **flow** handles all the incoming events and saves them into the underlay storage system. The events are differentiated by their types and their timestamp value. The flow is always active saving the events.

The **query** is dispatched by Intelie Live through a pipes filter wrapped in the **StorageQuery**, it receives the event type and the timestamp period to read events from. It reads the selected stored events and creates an **EventIterator** to deliver the data dynamically as necessary. The **EventIterator** typically will delegate the job to a cursor in the underlay storage system once it supports such feature.

The **deleteAll** and **delete** are available for integrators when they need to delete events. The **deleteAll** only requires the event type and prunes all the data associated. On the other hand, the **delete** enables the filtering by a pipes filter wrapped in the **StorageQuery** and applies only to the selected timestamp range.

The **allTypes** lists all the known event types that are saved in the underlay storage system. It is dispatched by Intelie Live once at the first initialization of the plugin and after the current quantity of event types is modified: new events saved by flow or when stored events are completely deleted.

### Additional resources

The **StorageQuery** class is described below. See the [storage-hints.md](../../pipes-queries/storage-hints.md "mention") page to learn about hints.

{% code title="StorageQuery.java" %}
```java
package net.intelie.live;

import net.intelie.pipes.filters.Filter;

public class StorageQuery {
    // some methods and constructors are hidden for simplicity

    // basic builders
    public StorageQuery withType(String type) {
    }

    // builders based on storage hints
    public StorageQuery withWhere(Filter where) {
    }

    public StorageQuery withNoSelect() {
    }

    public StorageQuery withSelect(String... select) {
    }

    public StorageQuery withLimit(int limit) {
    }

    public StorageQuery withTimestamp(String timestamp) {
    }

    public StorageQuery withFlags(Flag... flags) {
    }

    public StorageQuery withIntervalFilter(Filter intervalFilter) {
    }

    public StorageQuery withSpan(TimeSpan span) {
    }

    public StorageQuery withExtra(Map<String, Object> extra) {
    }

    // getters
    public String type() {
    }

    public Filter where() {
    }

    public Set<String> select() {
    }

    public int limit() {
    }

    public String timestamp() {
    }

    public int batchSize() {
    }

    public Set<Flag> flags() {
    }

    public TimeSpan span() {
    }

    public Filter getIntervalFilter() {
    }

    public Map<String, Object> extra() {
    }

    public enum Flag {
        NOCACHE, NOAUTOSELECT, NOMETRICS, NOCOUNT, NOASYNC, FORCEASYNC, NOINDEX, FORCECOUNT, REVERSED, FORCEINDEX;

        @Override
        public String toString() {
            return name().toLowerCase(Locale.ROOT);
        }
    }

}

```
{% endcode %}
