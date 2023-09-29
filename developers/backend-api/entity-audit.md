# Entity Audit

An **Entity Audit** represents a write operation on a **Live** entity, that was captured by a Hibernate Listener, and transformed into a new entity called `EntityAudit`. This new entity stores details of the operation and the new state of the entity in the system.

Live supports storing an `EntityAudit` in two forms: a _database record_ and/or an _event_ depending on the configuration set in the general system settings.

![configuration image](<../../.gitbook/assets/entity_audit_config.png>)

{% hint style="info" %} This feature is disabled by default. {% endhint %}

## Live auditable entities

There are some rules to an entity to be available for auditing. The `EntityAuditListener` is the base class of the listeners and apply some filters before auditing a entity:

- *The entity should not be annotated with `@IgnoreAudit`*. This annotation was created to mark entities that should be ignored by these audit listeners, e.g. `DashboardUserViewCount`.
- *The entity should not come from a call skipping audit*. This is used when the client calls the API method `Live.data().installEntities()` flagging to avoid auditing entities in a bulk entity creation.
- *The entity should not be a Setting nor a SettingLog*. This classes have a separate business logic and could possibly generate a deadlock at this point.

{% hint style="warning" %} **Delete operations do not store the complete content** of the entity due to a technical reason. In delete operations the Hibernate Listener can no longer have access to a relationship of the target entity. Another point is that this operation do not change the last version of the content being deleted and therefore would be a resource waste. {% endhint %}

## Plugin auditable entities

Starting in version 3.36.0, **Live** provides new API entry that permits plugins to register their own __Hibernate Session Factory__ into Live to expand the auditing features to the plugin's entities. Live will use the plugin's __Hibernate Session Factory__ to register a `EntityAuditDBListener` with all necessary `TypeAdapters` and used it to search for `EntityAudits` entities in the auditing endpoint (/rest/entity-audit). 

```java
/* This TypeAdapter is including an instance of EntityContext to provide a way to recovery the user by its ID */
EntityContext entityContext = live.data().getContext();
AssetTypeAdapter assetTypeAdapter = new AssetTypeAdapter(entityContext);
TypeAdapterDescriptor<AssetDTO, Asset> typeAdapterDescriptor = new TypeAdapterDescriptor<>("asset", Asset.class, assetTypeAdapter, AssetDTO::new);

EntityAuditOptions options = new EntityAuditOptions(Collections.singletonList(typeAdapterDescriptor));
live.data().auditHibernateSessions(sessionFactory, options);
```

It is also possible to extend the listener in order to customize the common rules and add more skip conditions to avoid auditing a write operation. With that in hand the plugin can register the listener by itself in its _Hibernate Session Factory_ as this snippet bellow:

```java
EntityAuditDBListener myListener = ...

EventListenerRegistry registry = ((SessionFactoryImpl) sessionFactory).getServiceRegistry().getService(EventListenerRegistry.class);
registry.getEventListenerGroup(EventType.POST_INSERT).appendListener(myListener);
registry.getEventListenerGroup(EventType.POST_UPDATE).appendListener(myListener);
registry.getEventListenerGroup(EventType.POST_DELETE).appendListener(myListener);
```

## Using audit as versioning

As said before, if the feature is enabled in the system configurations, the write operations of entities will be stored containing the current version of the entity in json format, e.g. the `Plugin` entity:

```java
@Entity
@Table(name = "plugin")
public class Plugin implements HasUpdateInfo {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "plugin_id")
    private Integer id;

    @Column
    private String name;

    @Column
    private String version;

    @Column
    private String relativePath;

    @Column
    private String sha;

    @Column
    private boolean active;

    @ManyToOne
    @NotFound(action = NotFoundAction.IGNORE)
    @JoinColumn(name = "author_user_id")
    protected User author;

    @ManyToOne
    @NotFound(action = NotFoundAction.IGNORE)
    @JoinColumn(name = "last_modification_author_user_id")
    protected User lastModificationAuthor;

    @Column(name = "date_created")
    private Long dateCreated;

    @Column(name = "date_modified")
    private Long dateModified;
...
```

will have a serialized value in json format of an `Plugin`

```json
{"plugin_id":73,"name":"plugin-oauth2","version":"4.0.0-SNAPSHOT","relativePath":"runtime/plugins/plugin-oauth2-4.0.0-SNAPSHOT.jar","sha":"521106f069e19cb655584fde535f94ea61de0c25","active":true,"date_created":1681149907570,"date_modified":1685978338791}
```

With this information in hand, it is possible to list the different versions of an entity's field (or all fields) and roll it back to a previous version.

{% hint style="info" %} Most entities will have all its fields parsed to json, but in some cases, e.g. `User`, this can not be done due to security reasons,like the password field that must not be exposed by the API.  {% endhint %}

### EntityAudit as an event

The `EntityAudits` can be listed as events querying by *type* `__audit` and *src* `core`

### EntityAudit as a database record

The `EntityAudits` stored as database records can be listed using the endpoints provided by the service `EntityAuditResource` available via `/rest/entity-audit`. There are three endpoints currently available:

#### List EntityAudit by type

This endpoint lists all `EntityAudits` of a specific entity, e.g. `{base-Url}/rest/entity-audit/list/perspective` will return a list of `EntityAudits` of the entity `Perspective`.

#### List EntityAudit by type and id

This endpoint lists all `EntityAudits` of a specific entity identified by *id*, e.g. `{base-Url}/rest/entity-audit/list/perspective/8` will return a list of `EntityAudits` of the entity `Perspective` having the *id* field 8.

#### Get EntityAudit content

This endpoint retrieves the entity content from an `EntityAudit`. It is used to obtain the entity stored by an `EntityAudit`, e.g. `{base-Url}/rest/entity-audit/100` will return the entity content of the `EntityAudit` with *id* 100. This method in intended to be used after the client have already chosen the specific `EntityAudit` containing the desired entity content.

If needed, the user can use the result of this method to roll back parts or all of the current version of an entity by calling the save method on the related resource class.

{% hint style="warning" %} Getting an EntityAudit that represents a delete operation will return a BAD_REQUEST. As explained before, EntityAudits of delete operations do not hold an entity content {% endhint %}

{% hint style="info" %} A change in version 3.36.0 was made in the list of EntityAudit returned by the listing endpoints (List by type and List by type and id). That change replaces the `author` field from a structure like `{"id":1, "name":"someone"}` to a simple integer representing the id. This was necessary to keep the model of EntityAudit from Live and the plugins the same as the plugins do not store users information to fill in the value.
