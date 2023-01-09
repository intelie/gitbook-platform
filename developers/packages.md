# Packages

A package is a plugin that defines a list of features (such as dashboards, datasources etc.) that can be installed by an Live admin user. It also defines a list of required identified channels and plugins. Its file structure is shown below.:

- live-plugins/plugin-my-package
  - pom.xml
    - src/main/resource
        - manifest.yaml

All packages require the plugin-package-manager. Without it, Live will not recognize a package plugin as a package and will not allow the admin user to install the features.

## The package manager plugin
The package manager plugin requires Live 3.29.0 or newer, and adds a new section to the admin screen. In this section, all installed packages are listed:
