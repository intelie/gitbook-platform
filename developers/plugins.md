# Plugins

Plugins can change Live behavior in many different ways, from creating a new visualization or integration to providing a completely new real time engine.

A plugin can be either:

* a JAR
* a ZIP
* a ZIP with a `manifest.json`
* a Groovy script
* a JDBC driver

{% hint style="info" %}
Most of the major features of Intelie Live, such as integrations with databases, web interface, and so on, are provided through plugins.
{% endhint %}

### JAR plugins

A Java plugin can contain Java and Javascript code, as well as code in any language that runs in the JVM, such as Groovy or Scala.

A Java plugin should depend on the [live-api](https://search.maven.org/search?q=live-api) library. See [Backend API](backend-api/) section for further information how to create a Java Live plugin. Plugins assembled as JAR plugins are also labeled FULL plugins.

### Zip plugins

A ZIP package can be used to provide static web content.

In that case, a ZIP may contain a folder called `webcontent`. The files in this folder will be provided at the URL`/content/plugin-name.zip/`. It's a useful approach to upload customer assets as fonts, icons, background images or JSON data files.

Plugins assembled as zip plugins are also labeled CONTENT\_NO\_MANIFEST plugins.

### Zip plugins with a manifest.json

Additionally to ordinary zip plugins a plugin can contain both `webcontent`folder as `manifest.json` to indicate to Live a set of manifest actions to perform. Plugins assembled as zip plugins including a manifes.json are also labeled CONTENT plugins.

Some examples of manifest actions supported in Live that promotes extensibility :

* **AddTag** : allows the dynamic load of HTML Tags (either CSS or JS) to Live HTML DOM. Arguments:
  * **position** : one of following string values HEAD\_BEGIN, HEAD\_END, BODY\_BEGIN, AFTER\_BODY\_HTML or BODY\_END
  * **cssFile** : (optional) JSON object containing `path` property to the css file location relative to plugin location (example: `/content/plugin-name.zip/custom.css`)
  * **cssSnippet** : (optional) JSON object containing `code`property with the stringified CSS content
  * **jsFile** : (optional) JSON object containing `path` property to the javacript file location relative to plugin location (example: `/content/plugin-name.zip/bundle.js`)
  * **jsSnippet** : (optional) JSON object containing `code`property with the stringified JS content
* **AddContent** : allows the dynamic load of some content inside the zip plugin to the Live URL path. Arguments:
  * **path** : string relative to Live plugin URL path that will be created
  * **location** : string relative to the contents of zip plugin where the file should be loaded from

See how these actions help the plugin development in [WebSetup](backend-api/web-setup.md) and [Live Widget Packaging](web-application/dashboard-and-widgets/live-widget-packaging.md) sections.

### Groovy scripts

A groovy file can be uploaded as a plugin. An example is shown below.

```groovy
//@liveplugin

import net.intelie.pipes.*
import net.intelie.live.*

import javax.servlet.http.HttpServletRequest
import javax.ws.rs.core.Context
import javax.ws.rs.*

@Path("/")
@Consumes("application/json")
@Produces("application/json")
class SomeService {
    def live
  
    SomeService() {}
    SomeService(live) {
        this.live = live
    }
  
    @GET
    @NeedsAuthority(Permission.ADMIN)
    def test() { 
      try {
          throw new RuntimeException()
          return live.engine().getQueryProviders().stream()
            .collect({[it.getKey(), Objects.toString(it.getValue().provider().info())]});
      } catch (Throwable e) {
          StringWriter errors = new StringWriter()
          e.printStackTrace(new PrintWriter(errors))
          return errors.toString()
      }
    }
}

live.web().addService('query-providers', new SomeService(live))
```

The comment `//@liveplugin` must be present at any line of the script in order to be understood as a plugin. Other special comments are supported as following:

* Plugin name and version, e.g.:`//@liveplugin MyVeryFirstService@1.0.0`
* Plugin dependencies, e.g.: `//@requirePlugins plugin-groovy, plugin-ldap;optional`
* Live platform version expected, e.g.: `//expectedLiveVersion 2.25`

Groovy scripts are an easier way to customize Live backend default behavior even the plugin mechanism itself. For instance, the developer can create its own manifest action with the following Groovy script:

```groovy
import net.intelie.live.*

class MyAction implements ManifestAction {
    int value = 42;
    
    void execute(Live live) {
        live.describeAction("Value: " + value, null);
    }
}

live.system().addManifestActionType("SomeAction", MyAction.class)
```

In the manifest.json the actions can be triggered passing some `value`to the action like:

```javascript
{
    "name":"SomeExample", 
    "version":"1.2.3", 
    "actions":[{"op":"SomeAction", "value":100}]
}
```

Plugins assembled as groovy scripts are labeled GROOVY plugins.

### JDBC drivers

A JDBC driver can be uploaded as a plugin and will be immediately available for use in any other plugin that depend on that connection.

Live currently supports that drivers: 

* Mysql
* Oracle
* Postgresql
* SQL Server
* SQLite
* DB2
