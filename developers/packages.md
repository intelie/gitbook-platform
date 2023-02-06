# Packages

A package is a plugin that defines a list of features (such as dashboards, datasources etc.) that can be installed by an Live admin user. It also defines a list of required identified channels and plugins. These definitions should be stored inside a manifest file, that can be either yaml or json. An example for a package source structure is shown below:

* live-plugins/plugin-my-package
  * pom.xml
    * src/main/resource
        * manifest.yaml

All packages require the [plugin-package-manager](https://marketplace.intelie.com/artifact/plugin-package-manager). Without it, Live will not recognize a package plugin as a package and will not allow the admin user to install the features.

At the moment that a package is installed, the features are not installed automatically. Some of them may require manual configuration. For example, before installing a dashboard, we need to define its perspective.

## The package manager plugin
The package manager plugin requires Live 3.29.0 or newer, and adds a new section to the admin screen. In this section, all installed packages are listed:

![Package screen](<../.gitbook/assets/image (175).png>)

Accessing a package, we can visualize its details, as well as activate and deactivate its features. There is also the possibility to inactivate the entire package, providing a way to hide all installed features at once, without losing its configuration.

![Hello World package](<../.gitbook/assets/image (176).png>)

Package manager plugin is not responsible for restricting access to installed objects, at the moment, installed entities will be responsible for permission semantics. At the moment, Live core features, as well as JS, CSS and Groovy snippets, cannot be edited when created by a plugin.

Packaging also allows the package creator to define a feature README (using markdown) and its version. It's also possible to register the changelog, but it isn't displayed at the user interface yet.

## Supported features
The package manager allows to create packages containing the following features:

- Dashboards
- Datasources
- Lookup Tables
- Pipes Modules
- Rules
- Groovy Snippets \*
- JS snippets \*
- CSS snippets \*

{% hint style="info" %}
IMPORTANT: If a package contains a JS or a CSS snippets it must require the [web snippets plugin](https://marketplace.intelie.com/artifact/plugin-websnippets). If it contains a Groovy Snippet, it should require the [plugin-groovy](https://marketplace.intelie.com/artifact/plugin-groovy/). See more about dependecies at required plugins section.
{% endhint %}  

## Extending plugin-package-manager to add more features
Package manager plugin exposes the `EntitiyRegistryService` service, that allows other plugins to register new entities types and, therefore, more features.

## Dependecies

### Required plugins
All packages require plugin package manager. Without it, Live will not recognize a package plugin as a package and will not allow the admin user to install the features.

As already mentioned, if a package contains a JS or a CSS snippet, it should require the [plugin-websnippets](https://marketplace.intelie.com/artifact/plugin-websnippets). If it contains a Groovy snippet, it should require the [plugin-groovy](https://marketplace.intelie.com/artifact/plugin-groovy/).

These dependencies should be described at the manifest file. The following example shows a package that depends on plugin-package-manager, plugin-websnippets and plugin-feed.

```
name: ${project.artifactId}
version: ${revision}
build: ${CI_PIPELINE_ID}
expectedLiveVersion: ${live-api-version}
requirePlugins:
  - plugin-package-manager
  - plugin-websnippets
  - plugin-feed
```

### Identified channels
Packages may require certain channel identifications to be configured in Live. This dependencies should also be described at the manifest, using the requiredEntities. For example:

```
name: ${project.artifactId}
version: ${revision}
build: ${CI_PIPELINE_ID}
expectedLiveVersion: ${live-api-version}
requirePlugins:
  - plugin-package-manager
  - plugin-websnippets
  - plugin-feed

actions:

  - op: packageFeatures
    package:
      title: Package Title
      uuid: 16335801-f46c-42db-a6d3-295f4f096ae4

      requiredEntities:
        - { type: "IDENTIFIED_CHANNEL", key: "standpipe_pressure" }
        - { type: "IDENTIFIED_CHANNEL", key: "rotary_speed" }
        - { type: "IDENTIFIED_CHANNEL", key: "torque" }
```

## Adding features

All features can be defined by the manifest file. A example is shown bellow:

{% hint style="info" %}
IMPORTANT: The version 4 UUID must be generate a must not be reused. 
{% endhint %}  

```
name: ${project.artifactId}
version: ${revision}
build: ${CI_PIPELINE_ID}
expectedLiveVersion: ${live-api-version}
requirePlugins:
  - plugin-package-manager
  - plugin-websnippets
  - plugin-groovy

actions:

  - op: packageFeatures
    package:
      title: Hello World
      uuid: dc4aed81-2e6c-4519-b18c-6143ec394431
      #icon: icon_48x48.png

      datasources:
        datasource_example:
          uuid: 42861930-a2e9-46ed-9d61-5ba02000dd81
          version: "1.0"
          content:
            title: "Packaging Example"
            identifier: "packaging_example"
            description: "Packaging example"
            text: "Rule text"
            expression:
              include: datasources/package_example.pipes

      rules:
        package_example:
          uuid: d9774736-17f0-4463-b375-a8c6b7c1d195
          version: "1.0"
          content:
            name: "Packaging Example"
            description: "Example rule"
            text: "Rule text"
            severity: INFO
            expression:
              include: rules/package_example.pipes


      dashboards:
        packaging_example:
          uuid: 629574b3-7327-4008-9fa0-3b0a3f06f941
          content:
            title: "Packaging Example"
            include: dashboards/packaging_example.json
          version: "2.0"
          readme: |
            A long remark regarding the member. Markdown syntax may be used for rich text
            representation and all. **Bold** and *italics* are supported.
            [Links](https://google.com/) go well, too. Remember that soft breaks are only
            made with two spaces at the end of the line so you can break lines normally on
            the manifest if needed.
            
            
            Separate paragraphs with a blank line between them.

            # Heading level 1

            1. list item
            2. list item
                1. nested list
                2. nested list
            3. list item

            ## Heading level 2

            - list item
            - list item
                - nested list
                - nested list
            - list item

            ### Heading level 3

            1. list item
                - mixed list

            #### Heading level 4

            - list item
                1. mixed list

            ##### Heading level 5

            > This is a blockquote.
            >
            > It can span multiple paragraphs like this.

            ###### Heading level 6

            ```
            This is a code block.
            ```

            C'est fini!
          changelog:
            2.0: |
              # Fixes and improvements
              - Added new widget Y
              - Improving widget X
            1.1: |
              # Fixes
              - Improving widget W


      pipes_modules:
        packaging_example:
          uuid: a60edaa0-e035-47ef-a5d2-8f765e6f002f
          version: "1.0"
          label: "Packaging example"
          content:
            expression:
              include: pipes-modules/packaging_example.pipes
          changelog:
            1.0: |
              # New features
              - Export common channels' aliases

      groovy_snippets:
        packaging_example:
          uuid: d858677c-7062-4bb5-95e6-20f0521ca085
          version: "1.0"
          label: "Packaging example"
          content:
            code:
              include: groovy-snippets/packaging_example.groovy

      js_snippets:
        packaging_example:
          uuid: b9208357-2e7b-42cf-898a-6d6264a391a7
          version: "1.0"
          content:
            position: HEAD_BEGIN
            code:
              include: js-snippets/packaging_example.js

      css_snippets:
        packaging_example:
          uuid: 8a73c60b-72e1-4dcf-bd4f-2bc279e56b85
          version: "1.0"
          content:
            position: HEAD_END
            code:
              include: css-snippets/packaging_example.css

      lookup_tables:
        packaging_example:
          uuid: 8244d8b8-89f8-48c2-a404-a46887824f1f
          version: "1.0"
          content:
            name: "Packaging Example"
            entries:
              - {key: "x", value: "1"}
              - {key: "hello", value: "world"}

        packaging_second_example:
          uuid: c700c38a-d67e-41dd-80fc-a5e5fabf5a07
          version: "1.0"
          content:
            qualifier: "packaging_second_example"
            name: "Packaging Second Example"
            entries:
              include: lookup-tables/packaging_second_example.json


      requiredEntities:
        - { type: "IDENTIFIED_CHANNEL", key: "standpipe_pressure" }
        - { type: "IDENTIFIED_CHANNEL", key: "rotary_speed" }
        - { type: "IDENTIFIED_CHANNEL", key: "torque" }
```

The `include` indicates the location of the feature content. So, for this example, the file structure will be: 

* live-plugins/plugin-my-package
  * pom.xml
    * src/main/resource
      * css-snippets/
        * packaging_example.css
      * dashboards/
        * packaging_example.json
      * datasources/
        * packaging_example.pipes
      * groovy-snippets/
        * packaging_example.groovy
      * js-snippets/
        * packaging_example.js
      * lookup-tables/
        * packaging_example.json 
      * pipes-modules/
        * packaging_example.pipes
      * rules/
        * package_example.pipes 
      * manifest.yaml
