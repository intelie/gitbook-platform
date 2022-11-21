# Web Setup

Live supports a easy way to put both backend and frontend together in a single plugin.&#x20;

For short, in backend maven project you could pick all web assets from something like `webapp/target` and made them available to `webcontent` folder inside the full plugin JAR as shown below.

```markup
<build>
    <resources>
        <resource>
            <directory>webapp/target</directory>
            <filtering>false</filtering>
            <targetPath>webcontent</targetPath>
        </resource>
        // ... other project resources you want
    </resources>
</build>
```

Live offers an util function  to register a `webcontent/bundle.js` as part of Live main application. You just need the following setup code at plugin startup.

```java
PluginUtils.defaultWebSetup(live);
```

Further information how to take advantage of Live to web development, please see the [Web Application](../web-application/) section.

### Appendix: PluginUtils

`PluginUtils.defaultWebSetup` is a static method that uses `Live.Web` interface to add a new HTML tag at very end of the main body Live tag including a script tag to `webcontent/bundle.js`.&#x20;

It's like magic, your web application just born!

```java
public abstract class PluginUtils {
    public static final String CONTENT_DIR = "webcontent";


    public static void defaultWebSetup(Live live) throws Exception {
        ClassLoader loader = live.system().classloader();

        URL contentUrl = loader.getResource(CONTENT_DIR);
        if (contentUrl == null)
            return;

        live.web().addContent("", contentUrl);

        URL bundleUrl = loader.getResource(CONTENT_DIR + "/bundle.js");
        if (bundleUrl == null)
            return;

        String resolved = live.web().resolveContent("bundle.js");
        live.web().addTag(HtmlTag.Position.BODY_END, new HtmlTag.JsFile(resolved));
    }

}
```
