# Web Services

Live follows JAX-RS specification to support RESTFul Web Services.

A plugin, an extension or even a Groovy Snippet just need use `Live.Web` interface to register a fresh new RESTFul resource as a service as described the below.

```java
live.web().addService("/tutorial", new SomeResource(live))
```

For example, the `SomeResource`can handle Live object as parameter to access the Settings API, seen in previous section, and provides a REST resource interface to some specific node.

```java
@Path("/")
@Produces("application/json")
public class SomeResource {

    private final String ships;

    public SomeResource(Live live) {
        this.ships = live.settings().root().cd("tutorial").raw();
    }

    @GET
    @Path("/ships")
    public JsonArray ships() {
        return LiveJson.fromJson(this.ships, JsonArray.class);
    }
}
```

The REST endpoint will be prefixed with the name chosen for your plugin, in this example a HTTP GET is allowed at`/services/plugin-tutorial/tutorial/ships`.

Live shows some details at plugin listings that enables the verification of which JAX-RS services were registered at plugin startup as shown below.

![Messages of recently uploaded plugin indicating JAX-RS service at some fresh new endpoint](<../../.gitbook/assets/image (131).png>)
