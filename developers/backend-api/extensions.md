# Extensions

Live Platform is extensible through different ways, one of most used forms we call Extensions and Live helps you to manage life cycle and configuration of extensions.

The installed extensions are managed through one of Integrations, Platform Customizations or Interface Customizations in Live administration menu.

![Different kinds of Extensions supported](<../../.gitbook/assets/image (41).png>)

The code below adds a very simple extension to Live Engine as part of a plugin starter code.

```java
public class Main implements LivePlugin {
    @Override
    public void start(Live live) throws Exception {
        live.engine().addExtensionType(new SomeType(live));
        PluginUtils.defaultWebSetup(live);
    }
}
```

## Type and Configuration

An extension is defined by a type and an configuration classes that should implements `ExtensionType` and `ExtensionConfig` interfaces respectively, as shown below.

{% tabs %}
{% tab title="SomeType.java" %}
```java
public class SomeType implements ExtensionType<SomeConfig> {

    private final Live live;

    public SomeType(Live live) {
        this.live = live;
    }

    @Override
    public String typename() {
        return "plugin-tutorial";
    }

    @Override
    public Set<ExtensionRole> roles() {
        return ExtensionRole.INPUT.asSet();
    }

    @Override
    public ExtensionArea area() {
        return ExtensionArea.PLATFORM;
    }

    @Override
    public ElementState test(ExtensionQualifier qualifier, SomeConfig config) 
    throws Exception {
        return ElementState.OK;
    }

    @Override
    public ElementHandle register(ExtensionQualifier qualifier, SomeConfig config)
    throws Exception {
        return SafeElement.create(live, qualifier, 
                   (prefixed, qual) -> new ElementHandle.Default(prefixed));
    }

    @Override
    public SomeConfig parseConfig(String config) throws Exception {
        return LiveJson.fromJson(config, SomeConfig.class);
    }
}
```
{% endtab %}

{% tab title="SomeConfig.java" %}
```java
import com.google.gson.JsonArray;

public class SomeConfig implements ExtensionConfig {
    private JsonArray ships;

    @Override
    public String summarize() {
        return "An extension to fullfil the ships information";
    }

    @Override
    public Set<ExtensionRole> roles() {
        return ExtensionRole.INPUT.asSet();
    }

    @Override
    public ValidationBuilder validate(ValidationBuilder validationBuilder) {
        return validationBuilder.requiredNotNull(ships, "ships");
    }
    
    public JsonArray getShips() {
        return this.ships;
    }
}
```
{% endtab %}
{% endtabs %}

TIP: Live provides a helper class `LiveJson` to ease translate a JSON string to class attributes and automatically fulfill the `SomeConfig.ships` attribute in our example above.

### Naming

The string returned by `typename`method will represent the Live element in Live Engine, e.g. if the extension provides a REST service the typename will prefix the url location of it. Give some love to it.

### Area

Extensions can be hosted in Live under the main areas `INTEGRATION`, `PLATFORM`or `CUSTOMIZATION`, as previously seen in screenshot at top of this section.

### Roles

Extensions can be organized using a set of roles to ease find them on the Live marketplace. The following roles are supported:&#x20;

* `INPUT`: indicates that extension could receive data or event and makes them available in Live.&#x20;
  * Built-in: ActiveMQ, Phone Sensors, Replay, REST and TCP.&#x20;
* `NOTIFICATION`: indicates that extension could notify live activities to external applications.
  * Built-in: ActiveMQ and SMTP.
* `QUERY`: typically used by query providers to operate alongside pipes.
  * Built-in: Pipes, Esper, Groovy Snippets, Lognit, MongoDB and SQL.
* `STORAGE`: when the extension saves live events in some data stores.
  * Built-in: MongoDB, PostgreSQL and SQL.
* `UI`: customizes how Live UI looks.
  * Built-in: Dashboard Templates, CSS Files, CSS Snippets, JS Files and JS Snippets.&#x20;
* `AUTH`: adds some other authentication method to Live.&#x20;
  * Built-in: LDAP, OAuth 2.0 and SAML 2.0.
* `OUTPUT`: configures an output for a event query.

TIP: Extension roles are optional, just organizational purposes, e.g. there's another built-in extension that doesn't use any of these roles: Lookup Tables.

### Qualifier

Extensions are qualified by a string provided at configuration time, typically when Live administrator fulfill the parameters required for an ExtensionConfig concrete class. The extension developer can handle the qualifier received in `register` and `test`methods to identify different instances of an extension.

Qualifiers in Live must be unique but Live also allows the existence of an empty qualifier. Remember that multiple qualifiers are organized under the same extension type.

## Life-cycle and Management

Live supports multiple instances of a same extension. It's useful to read from many storage providers or to receive data from any arbitrary number of REST input endpoints and process all data together from event processing capabilities in Live, e.g. using Pipes queries.

After compile and package your very first tutorial plugin, you may upload it using Plugins menu in Live administration menu.

![Example of plugin-tutorial recently uploaded to Live platform](<../../.gitbook/assets/image (149).png>)

The tutorial plugin just adds an extension type, so you can check the message saying "_added extension type 'plugin-tutorial' as PLATFORM_" in the plugin list screen.&#x20;

Now, let's go find it under the Platform Customization area in Live administration.

![Extension instance management interface](<../../.gitbook/assets/image (83).png>)

### Add instance

Extension instance configuration provides a basic form to fulfill the qualifier and a JSON that will be serialized as string and given as argument to concrete version of`ExtensionType.parseConfig` method.

![New instance configuration form of the tutorial extension](<../../.gitbook/assets/image (168).png>)

### Validation and Save

The Live default form provides two form buttons **Test** and **Save**.&#x20;

**Test** action will trigger a POST to Live internal endpoint `/rest/extension/test` using data provided on the form provided and also the plugin metadata (e.g. typename).&#x20;

Live will automatically dispatch the method call to the concrete version of `ExtensionConfig.validate` in order to validate the configuration provided for that instance described by that qualifier. So, developer may check if the config attributes meet its requirements returning a `ValidationBuilder` object.

Once validated, the Live will represent the validation results as `VALID` or `INVALID`.

![Example of a VALID instance configuration](<../../.gitbook/assets/image (9).png>)

For example, our `SomeConfig.validate` method required the ships to be a not null attribute. In case of missing the fulfill of `ships` JSON field it remains null and validation will fail.

![Example of an INVALID instance configuration (missing ships field)](<../../.gitbook/assets/image (91).png>)

**Save** action will trigger a POST to another Live internal endpoint `/rest/extension` in order to save the new instance itself. The mechanics of validation and configuration parsing is the same but now Live creates a new element in Live Engine and calls `ExtensionType.register` to proceed the developer code on the fresh new instance created.

At this time, the extension instance has an unique ID in Live subsystems and further operations as instance stop or plugin stop will be handled by Live itself.

### Stop and Remove

Extension instances can be stopped and removed from Live through its own listing on Live area. The same where you trigger the creation of the instance.

![Actions to stop and remove the instance of an extension](<../../.gitbook/assets/image (108).png>)

## Appendix: ElementHandle, ElementState and ElementStatus

In detail, `ExtensionType.register` method must return a `ElementHandle` object.&#x20;

`ElementHandle` is a abstract class that enables the extension to inform its `status` and Live calls `refresh` periodically (extension should update its status at that calls) and delegates the termination of an instance through `close` method. The code below shows a short version and a complete version (with helper inner classes) of this class.

{% tabs %}
{% tab title="ElementHandle (short version)" %}
```java
public abstract class ElementHandle {

    public void refresh() {}

    public void close() {}

    public abstract ElementState status();
}
```
{% endtab %}

{% tab title="ElementHandle (complete version)" %}
```java
public abstract class ElementHandle {
    public static final Constant INACTIVE = new Constant(ElementState.INACTIVE);
    public static final Constant OK = new Constant(ElementState.OK);

    public void refresh() {}

    public void close() {}

    public abstract ElementState status();

    // additional inner classes to ease some stuff

    public static class Default extends ElementHandle {
        private final PrefixedLive live;

        public Default(PrefixedLive live) {
            this.live = live;
        }

        @Override
        public ElementState status() {
            LinkedHashMap<String, Object> metrics = new LinkedHashMap<>();
            metrics.put("actions", live.actions().stream().map(action ->
                    new MapBuilder()
                            .put("prefix", action.prefix())
                            .put("description", action.description())
                            .put("url", action.url()).ok()).collect(Collectors.toList()));

            return new ElementState(ElementStatus.VALID, metrics);
        }

        @Override
        public void close() {
            live.undoAll();
        }
    }

    public static class Constant extends ElementHandle {
        private final ElementState status;

        public Constant(ElementState status) {
            this.status = status;
        }

        @Override
        public ElementState status() {
            return status;
        }
    }

    public static class Merged extends ElementHandle {
        private final ElementState status;
        private final ElementHandle other;

        public Merged(ElementState status, ElementHandle other) {
            this.status = status;
            this.other = other;
        }

        @Override
        public ElementState status() {
            return status.merge(other.status());
        }

        @Override
        public void close() {
            other.close();
        }

        @Override
        public void refresh() {
            other.refresh();
        }
    }
}
```
{% endtab %}
{% endtabs %}

The status of an extension (actually of any Live element) is represented by `ElementState`and can assume a sort of states: `OK`, `UNKNOWN`, `UNSUPPORTED`, `INACTIVE`, `STARTING`, `TIMEOUT`, etc.

These states provide some additional information at top of a basic `ElementStatus` enumeration. In turn, `ElementStatus` could assume one of `VALID`, `VALID_BUT`, `INVALID` or `INACTIVE` statuses. Just `VALID` means `true` and the others mean `false`.

The most used states are `OK` (`VALID` status) and `INACTIVE` and `STARTING` (`INVALID`statuses).&#x20;

For instance, if something goes wrong at registration phase (or even in status method) you could return something like `new ElementState(ElementStatus.INVALID, "with some message")`.

## Appendix: Saving ships as Settings

Let's make a simple modification to our tutorial Extension that enables the persistence of `ships` data using [Settings](settings.md) for didactic purposes of topics covered in [Web Services](web-services.md) section.

{% code title="SomeType.java" %}
```java
// ...
@Override
public ElementHandle register(ExtensionQualifier qualifier, SomeConfig config) 
throws Exception {
    return SafeElement.create(
        live, qualifier, (prefixed, qual) -> {
            prefixed.settings().root().cd("tutorial").set(config.getShips());
            return new ElementHandle.Default(prefixed);
        }
    );
}
// ...
```
{% endcode %}

## Appendix: Finding all extensions created

In case you need to list all extensions created, you may use the `live.data().getContext()` API and the `AllExtensions` Live facility as the criteria to query the underlay SQL database. The Live package `net.intelie.live.queries` delivers other similar facilities for all Live basic entities (e.g dashboards, datasources, plugins, perspectives, etc).

```java
//...
import net.intelie.live.EntityContext;
import net.intelie.live.queries.AllExtensions;
//...
// Live API EntityContext wraps the access to Live model persistence
EntityContext entityContext = live.data().getContext();
// list all extensions configured (active or inactive) in the Live runtime
entityContext.find(new AllExtensions());
//...
```

The `EntityContext` API and Live Data API will be discussed in details in another section.

{% hint style="info" %} Frontend plugins have no such API to list extensions (or generic entities). Each resource in Live REST API is protected according. For example, the `/rest/extension` endpoint list all extensions but it is restricted to Live administrative users only.  {% endhint %}

