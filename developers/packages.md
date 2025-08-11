# Packages

A package is a plugin that defines a list of features (such as dashboards, datasources etc.) that can be installed by an LIVE admin user and a list of required entities and plugins. These definitions should be stored inside a manifest file, which can be either yaml or json. An example of a package source structure is shown below:

* live-plugins/plugin-my-package
  * pom.xml
  * src/main/resource
    * manifest.yaml

All packages require the [plugin-package-manager](https://marketplace.intelie.com/artifact/plugin-package-manager). Without it, LIVE will not recognize a package plugin as a package and will not allow the admin user to install the features.
