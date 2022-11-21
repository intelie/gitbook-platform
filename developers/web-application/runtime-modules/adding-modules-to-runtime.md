---
description: >-
  Any script can extend the runtime module system, Live offers a utility
  function to expose a directory of javascript modules.
---

# Adding modules to runtime

### The directory structure

It is advised to create a **shared** folder within your project, like so:

![Suggested directory structure](<../../../.gitbook/assets/image (50).png>)

### Exposing the entire "shared" directory

Inside the **shared folder** create **index.ts** file with the following content:

```javascript
// exposeRequireContext depends on webpack's requireContext
import { exposeRequireContext } from 'live/expose'


// #see: https://webpack.js.org/guides/dependency-management/#requirecontext
const req = require.context(
    './', // require from current directory
    true, // recursive
    // it is important to ignore some folders and files
    /^(?!.*__tests__.*)^(?!.*\.stories\.*)^(?!.*Spec.js)^(?!.*worker.js)^(?!.*__mocks__.*\.js)((.*\.([t|j]sx?\.*))[^.]*$)/
)

// this call expose recursively all the modules on the current folder
// second argument is the prefix of the modules
exposeRequireContext(req, 'plugin-example/shared/')

```

### Import the shared index file from the app entry point

{% code title="app.tsx" %}
```javascript
import '../shared'
// ... rest of the app.tsx file
```
{% endcode %}

That's it! Your modules are ready to be required by another plugin's web application.

### Example result

![](<../../../.gitbook/assets/image (41).png>)

Before importing from `plugin-example` there are some configurations needed to be set up.

### 1. Configure your `webpack.config.js` to treat these modules as external

{% code title="webpack.config.js" %}
```javascript
...
    externals: [
        function(context, request, callback) {
            // tells webpack that plugin-example is an external module
            if (request.startsWith('plugin-example/'))
                return callback(null, 'commonjs ' + request)

            callback()
        }
    ]
...
```
{% endcode %}

### 2. Configure your `pom.xml` to add explicitly show that dependency

```markup
...
<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-jar-plugin</artifactId>
    <version>3.0.2</version>
    <configuration>
        <archive>
            <manifestEntries>
                <name>${project.name}</name>
                <version>${project.version}</version>
                <build>${buildID}</build>
                <author>Intelie</author>
                <entryPoint>net.intelie.example.plugin.pioneer.Main</entryPoint>
                <!-- explicit depdendency to plugin-example -->
                <requirePlugins>plugin-example@1.1.2,plugin-another-example@1.1.2</requirePlugins>
            </manifestEntries>
        </archive>
    </configuration>
</plugin>
...
```

Now you're set up to import modules from **`plugin-example`!**&#x20;

### 3. Import from `plugin-example`  in any module

```javascript
import util from 'plugin-example/shared/util'
```
