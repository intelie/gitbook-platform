# Live 4
## 4.0.6 (November 24, 2023)

### New features

* Remove call of renderGroupedYAxis in Widgets
* Fixes a bug that allows creating widgets without a name
* Update the react-select library in live 4

### Bugfixes

* Live does not expose d3 submodules
* Export widget exception error

## 4.0.5 (November 10, 2023)

### New features

* Fix dashboard span type config
* [LIVE4] Temporal and Cartesian charts does't work when render first time outside dashboard context

### Improvements

* Provide all runtime live-ui dependencies as "externals / library modules"

### Bugfixes

* [LIVE4] BASIC WIDGET JSX DOESN'T WORK
* Avoid calling the span multiple times

## 4.0.4 (November 3, 2023)

### New features

* Override vulnerability of xmlsec dependency on plugin-samlv2
* Vulnerability / Cross-Frame Scripting
* Follow-up from "Expose deadlock checker endpoint and add built-in watchdog"
* Put dashboard change span type option in more options config
* Fix plugin vulnerabilities

### Bugfixes

* Fix error when opening a Pipes Modules

## 4.0.3 (October 20, 2023)

### New features

* Vulnerability / Often Misused: HTTP Method Override
* Fix publish-live-ui-api
* Remove LiveApi.Interceptor.dashboard usage

### Bugfixes

* Expose the following react-select components: animated, async, async-creatable, base, creatable
* Fix currentUser
* Fixes a bug that shows a blank screen when loading Live 4.0.x

## 4.0.2 (October 6, 2023)

### New features

* Dashboards no longer receives modifications
* Enable Entity Auditing in External Hibernate Sessions
* Use versioned dependency for Java
* LiveSelect label should be associated with Select

### Bugfixes

* Fixes LiveSelect creatable prop
* Fixes a bug that prevents JSX widgets from render correctly
* Fixes a bug that shows an error when the user changes the type of a pipes chart
* Fix the UserPresence feature

