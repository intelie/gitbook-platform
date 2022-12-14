# Live 3

{% hint style="info" %}
We advise that, from July of 2021 on, `systemd` will be a mandatory dependency for Live. This way, new versions will only be installed on operational systems that support `systemd.`
{% endhint %}

{% hint style="info" %}
Beginning in this version Live will start to use the [Semantic Versioning 2.0.0](https://semver.org/). &#x20;
{% endhint %}

## [3.27.0](3.27.0.md) (November 11, 2022)

### New Features

* Allow setting the number of ticks on y axis even if the axes extremes are not defined
* Move jersey-multipart from live-core to live-api
* UI visibility Configurations

### Fixes and improvements

* Fix browser not unsubscribing to cometd channels on unload

## 3.26.3 (November 04, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.25.3

## 3.25.3 (November 04, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.24.6

## 3.24.6 (November 04, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.23.6

## 3.23.6 (November 04, 2022)

### Fixes and improvements

* Fix sometimes changing only one of the y axis extremes in temporal widgets is not reflected in the chart
* Fix Live alert adding condition to accept strings from 1 to 9&#x20;
* Fix widget does not wait for web worker to be initialized

## 3.26.2 (October 28, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.25.2
* Fix API break at AllSettings and AllSettingLogs

## 3.25.2 (October 28, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.24.5

## 3.24.5 (October 28, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.23.5

## 3.23.5 (October 28, 2022)

### Fixes and improvements

* Fix data not being shown in track title on mouse hover
* Fix widget view components breaks at remounting

## 3.26.1 (October 14, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.25.1

## 3.25.1 (October 14, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.24.4

## 3.24.4 (October 14, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.23.4

## 3.23.4 (October 14, 2022)

### Fixes and improvements

* Simplify PurgeTask to remove the multiple attempts
* Fix ConcurrentModificationException breaks shifted queries
* Fix Widget download resource is not setting CTX\_USER or timezone on the Query Context
* Change from generic select to LiveSelect component in Lookup Table page
* Analyze the post conditions which cause the PurgeTask to finish in failed state due count mismatches

## 3.24.3 (October 6, 2022)

### Fixes and improvements

* Fix darkmode of find-all popup
* Fix shifted query stopping after error

## [3.26.0](3.26.0.md) (September 30, 2022)

### New Features

* Updated Pipes to 0.25.5
  * Update introspective to 0.12
  * Cron should not consider leading spaces&#x20;
  * Fix NPE at stateful pipe within transform operator
  * Java 17 fixes&#x20;
  * Update dependencies of pipes-tutorial&#x20;
  * Start of nullability annotations
* Improved code documentation (javadoc) on Live API
* Added crop function on service span from Live
* New API to Manage Live Entities from Plugins

### Fixes and improvements

* Improvements on storage statistics administrative UI to inform data allocation details
* Fixed cancel queued work on plugin executor shutdown

## [3.25.0](3.25.0.md) (August 26, 2022)

### Fixes and improvements

* Arrow in timestamp hint is now accepted;
* Fixed filter plugin LookupTable content based on user's permission;
* Fixed widget edit page had some style problems when dark mode is enable;
* Cumulated changes from patch 3.24.2;

## 3.24.2 (August 26, 2022)

### Fixes and improvements

* Fixed error on queries without type filter;
* Cumulated changes from patch 3.23.3;

## 3.23.3 (August 26, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.22.4;

## 3.22.4 (August 26, 2022)

### Fixes and improvements

* Fixed query status shown as OK when dropping events;
* Fixed reducer was ignored when downloading data through the console page;

## 3.24.1 (July 15, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.23.2

## 3.23.2 (July 15, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.22.3

## 3.22.3 (July 15, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.21.5

## 3.21.5 (July 15, 2022)

### Fixes and improvements

* Fixed reprocessing failed queue not working if events reached maximum retry count;
* Fixed form-group component not receive disabled props;
* Fixed marker configuration by pipes modifiers not working;
* Fixed error setting badge. Message: Too many badges requests in queue;
* Fixed responsiveness issue on the dashboard menu in view mode;

## [3.24.0](3.24.0.md) (July 01, 2022)

### New Features

* Isolate window advance from all unrelated queries
* Add daily consolidated metrics to Live
* Send Daily Consolidated Metrics Data to Operations
* Add configuration to expire user accounts after a given date
* Add menu to console icon on header
* Move ChangeLog to Plugins Main Page
* Add menu to console icon on header
* plugin-messenger: Use palette V2 in Live

### Fixes and improvements

* Add new service to calculate number of ticks
* Remove buffer old entries in web worker

## 3.23.1 (July 01, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.22.2

## 3.22.2 (July 01, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.21.4

## 3.21.4 (July 01, 2022)

### Fixes and improvements

* Fix select component "disabled" prop has no effect
* Fix dashboard Performance Report has incorrect style when dark mode is enabled
* Fix dashboard scrolling is covering widgets at the end
* Fix dashboard menu opens when typing 'n' twice when user is writing text
* Fix duplicate GET requests in list page
* Allow disabling highlight points by service
* Remove the limited height when visualizing an expression at the Dashboard Performance Report
* Missing translations at "chart options" menu
* Improve the readability of amount of bytes at purge details
* Enable purge queries to count dynamically

## [3.23.0](3.23.0.md) (June 03, 2022)

### Fixes and improvements

* Create an extensibility for plugins admin header with a registry
* Expose the listenerQueue count on the query stats

## 3.22.1 (May 20, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.21.3

## 3.21.3 (May 20, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.20.5

## 3.20.5 (May 20, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.19.3

## 3.19.3 (May 20, 2022)

### Fixes and improvements

* Improved the animation on the widget when expanding or restoring
* Fixed incorrect font color in query output display

## [3.22.0](3.22.0.md) (April 29, 2022)

### Fixes and improvements

* Improved usability of widget charts
* Cumulated changes from patch 3.21.2

## 3.21.2 (April 29, 2022)

### Fixes and improvements

* Cumulated changes from patch 3.20.4

## 3.20.4 (April 29, 2022)

### Fixes and improvements

* Fixed and error in reset password page
* Cumulated changes from patch 3.19.2

## 3.19.2 (April 29, 2022)

### Fixes and improvements

* Fixed a bug on date-pickers on fresh Live installs
* Fixed charts tooltip not displaying right colors&#x20;
* Fixed an error on widget "More Options" button
* Fixed a bug on lookup tables being incorrectily non editable

## 3.21.1 (April 11, 2022)

### Fixes and improvements

* Fixed an internal problem on the build process.&#x20;

## [3.21.0](3.21.0.md) (April 08, 2022)

This version is not available for installation due to an internal problem on the build process. Please install 3.21.1 instead.

### New features

* Integrate @intelie/ui Switch component
* Internal: published docker image now supports linux/amd64 and linux/arm64. The reference `tools/docker` script was updated to reflect new required flags when running the container.&#x20;

### Fixes and improvements

* Fixed an error at subscription service when subscribing to a channel that was closed
* Fixed concurrency problems

## 3.20.3 (April 08, 2022)

### Fixes and improvements

* Cumulative changes from 3.19.1.

## 3.19.1 (April 08, 2022)

### Fixes and improvements

* Cumulative changes from 3.18.3.

## 3.18.3 (April 08, 2022)

### Fixes and improvements

* Add workaround for CVE-2022-22965
* Bug on plugin samlv2 including copy of slf4j-api
* FormGroup trashes numeric values that are over 1000
* Widget span type modal in Live's dark shows a wrong background color
* Grouped axis of a chart shows an incorrect background color on Live's dark mode
* Chart legends is illegible in dark mode
* Dashboard page dark mode visual fixes pack
* Implement support for detecting iPad from iOS > 13 as tablet
* Lookup table dark mode style fix
* Refresh button style on widget edit page has a wrong background color
* Default pipes widget is not aware of interceptors when downloading
* Fix bug displaying Available rooms into the messenger sidebar
* Missing build number in live-ui manifest
* Filter options selected filter jumps the first line when its name is too long

## 3.20.2 (March 25, 2022)

### Fixes and improvements

* Temporarily disabling some slf4j classes in PluginValidator to prevent problems in plugins that already include these classes.
* Fixed a bug in standalone widgets
* Cumulative changes from 3.18.2.

## 3.20.1 (March 25, 2022)

This version has been deprecated due to a technical problem on the release process.

## 3.18.2 (March 25, 2022)

### Fixes and improvements

* Fixed a deadlock bug in specific scenarios
* Bug on plugin samlv2 including copy of slf4j-api
* Plugin samlv2 including copy of slf4j-api
* Implement support for detecting iPad from iOS > 13 as tablet
* Refresh button style on widget edit page has a wrong background color
* Default pipes widget is not aware of interceptors when downloading
* Fix bug displaying Available rooms into the messenger sidebar
* Missing build number in live-ui manifest
* Filter options selected filter jumps the first line when its name is too long&#x20;

## 3.18.1 (March 25, 2022)

This version has been deprecated due to a technical problem on the release process.

## 3.16.2 (March 17, 2022)

### Fixes and improvements

* Fix a bug when saving filtered lookup tables

## [3.20.0](3.20.0.md) (March 11, 2022)

### New features

* Expose DataGaps Report Threshold configuration
* The main persistent queue can be configured through live.properties and UI
* Allow configuration of safe and fail queue size in admin page
* Add widgetTransientParams in LiveWidgetRequest.userConfig
* Highlight the line to show data points in temporal charts

## [3.19.0](3.19.0.md) (February 25, 2022)

### New features

* Allow groovy snippets to use JAR libraries from the local filesystem
* Update jquery-ui dependency

### Fixes and improvements

* During the first few seconds of loading the dashboard, all widgets appear to be on top of each other
* NullPointerException at ObservableSettingsProvider unregister with missing slash

## [3.18.0](3.18.0.md) (February 11, 2022)

### New features

* \[API] New backend API that exposes locale information

### Fixes and improvements

* Move Annotations metrics to plugin-annotations
* Improve UX for Live theme modal
* \[fixed] Widget footer is above depth span picker
* \[fixed] When downloading a widget, the user local customization should be used

## [3.17.0](3.17.0.md) (January 28, 2022)

### New features

* Optional dependency for plugins ([more](3.17.0.md#weak-optional-dependency-for-plugins))
* Live will reject plugins that contain copies of some JARs/Libraries ([more](3.17.0.md#live-will-reject-plugins-that-contain-copies-of-some-jars-libraries))

### Fixes and improvements

* Add new tabs(message, warning, dependencies) on plugins screen
* Associate status Warning for plugins with version snapshot and development builds
* Fix dashboard filter ordering of lookup table items

## 3.16.1 (January 25, 2022)

### Fixes and improvements

* \[fixed] Live theme stops providing old variables at times
* \[fixed] "Chart Options" of Dashboards and Console have some strange colors on light and dark themes
* \[fixed] Fixing sort by initialization time at plugin screen

## [3.16.0](3.16.0.md) (January 14, 2022)

### New features

* \[beta] Per-device theme customization

### Fixes and improvements&#x20;

* UI/UX improvements in the failsafe interface
* Healthcheck | Add Config and Utils to public API
* Improvements for SAML login
* \[fixed] Download: Merge rows with the same timestamp not working in csv and excel



## 3.15.0 (December 30, 2021)

### New features

* Plugin healthcheck | Other plugins can now extend the Monitor list ([more](3.15.0.md#plugin-healthcheck-or-other-plugins-can-now-extend-the-monitor-list))
* New lookup tables UI ([more](3.15.0.md#new-lookup-tables-ui))
* SMTP integration | Allows specifying a set of protocols enabled for SSL connection ([more](3.15.0.md#smtp-integration-or-allows-specifying-a-set-of-protocols-enabled-for-ssl-connection))

### Fixes and improvements

* \[fixed] First login with external authentication fails to identify an existing user with the same email
* \[fixed] Header size does not match the number of axes in vertical
* \[fixed] Some channels appear multiple times in the grouped tooltip
* \[fixed] Tooltip without background on the temporal pipes widget
* \[fixed] Legends being kept when new filter changes at the bar chart
* UI improvements at dashboard filter setup
* UI improvements at widget header
* Multiple UI/UX improvements at plugin admin screen

## [3.14.0 (December 17, 2021)](./#3.14.0-december-17-2021)

### Fixes and improvement <a href="#user-content-fixes-and-improvements" id="user-content-fixes-and-improvements"></a>

* Include plugin count at the plugin admin interface&#x20;
* Include plugin upload progress at the plugin admin interface&#x20;
* \[fixed] First setup of multiple filters is tied together
* \[fixed] Dropdown configurations menu expands out of screen area
* \[fixed] Invalid extension name can lead to a "No such servlet" error
* \[fixed] Console interface is breaking with the periodic provider
* \[fixed] Failure expanding larger \_\_type strings in Live web console
* \[fixed] Unable to expand pipes module idle horizontally
* \[API] Update guava to 31.0.1
* \[API] Update gson to 2.8.8
* \[API] Update kotlin to 1.5.31
* \[API] Update JetBrains.Annotations to 22.0.0
* \[API] Add ahooks 3.0.0 library&#x20;
* \[API] Include web services final URLs to the Extension Config API&#x20;

## 3.13.2 (December 13, 2021)

### Fixes and improvement <a href="#user-content-fixes-and-improvements" id="user-content-fixes-and-improvements"></a>

* \[fixed] Header size does not match the number of axes in the vertical orientation
* \[fixed] Wrapped channel appears multiple times in the grouped tooltip with the group by x option
* More consistent detection of drag action in the header to order the channels

## 3.13.1 (December 10, 2021)

### Fixes and improvement <a href="#user-content-fixes-and-improvements" id="user-content-fixes-and-improvements"></a>

* \[fixed] Individual tooltip is not working

## [3.13.0 (December 03, 2021)](3.13.0.md)

### Fixes and improvement <a href="#user-content-fixes-and-improvements" id="user-content-fixes-and-improvements"></a>

* \[fixed] Query statistics metrics can still generate huge events
* \[fixed] When pipes documentation text is large, one cannot click on the documentation link in the autocomplete
* \[fixed] Widget edit page horizontal scroll
* \[fixed] Datasource pipes query editor unreliably expands over the data output area
* \[fixed] Label channels on dark mode were always white
* \[fixed] Units are displayed in the wrong color in the pipes widget header
* Performance improvement&#x20;

## 3.12.1 (December 09, 2021)

### Fixes and improvement <a href="#user-content-fixes-and-improvements" id="user-content-fixes-and-improvements"></a>

* \[fixed] Units were displayed in the wrong color in the pipes widget header&#x20;
* \[fixed] Label channels on dark mode were always white&#x20;
* \[fixed] Individual tooltip is not working
*   \[fixed] Console interface is breaking with the periodic provider

    &#x20;

## [3.12.0 (November 19, 2021)](3.12.0.md)

### Fixes and improvements <a href="#user-content-fixes-and-improvements" id="user-content-fixes-and-improvements"></a>

* Rules interface now shows the  "provider" on every item
* \[fixed] Some of the sub-menu items are hiding on mouse over
* \[fixed] Truncated filters should show a tooltip when hovering any part of it
* \[fixed] Codemirror is not working on Firefox for Mac
* \[fixed] Empty widget is not draggable
* \[fixed] Channel value is not showing in the header when the series is out of the y-axis
* \[fixed] The axis header does not show values after change configuration
* \[fixed] User is not able to click 'restore default' to make the widget follow the dashboard's span
* \[fixed] Axis does not receive properties correctly after ordering channels
* Plugin interface
  * Add option to sort plugins
  * UI/UX improvements
  * Enable install a plugin by drag and drop a file on all page

## [3.11.0 (November 05, 2021)](3.11.0.md)

### New features

* Vertical Temporal Xrange chart ([more](3.11.0.md#vertical-temporal-xrange-chart))

### Fixes and improvements

* Include more information in the query metrics event
* \[fixed] Possible infinite loop to create Storage Queries
* \[fixed] Tooltip behind the Temporal XRange timeline
* \[fixed] New plugin screen can break at plugin update
* Console UI/UX improvement
  * Query field should have a maximum width with a title helper
  * \[fixed] Long message breaks query loading layout

## [3.10.0 (October 22, 2021)](3.10.0.md)

### New features

* New Plugin Admin interface ([more](3.10.0.md#new-plugin-admin-interface))
* Ability to change the position of each series in the widget header ([more](3.10.0.md#ability-to-change-the-position-of-each-series-in-the-widget-header))

### Fixes and improvements

* Update pipes to 0.25.2
  * Ignore I/O error in sys.disks
  * Fix UnionPipe and JoinPipe behavior with empty batches
  * Fixed Exprange for step equals to 1
  * Exprange should be finite
* \[fixed] Failsafe screen should not show PANIC buttons to non-Admins&#x20;
* \[API] Deprecate redo() and undo() from Live.Action
* Add "final computed span" to \_\_queries event
* Performance improvement to Lookup Table interface

## [3.9.0](3.9.0.md) (October 8, 2021)

### New features

* Add the custom series title to the bubble chart
* \[API] New UI Widget Extension Points

### Fixes and improvements

* \[fixed] Plugin start duration - Format long time periods using different time units (ms, s, m, etc)&#x20;
* \[fixed] `__audit` event has incomplete data&#x20;
* \[Ops] Allow starting a new log file without restarting Live
* Optimize classloader use
* Keyboard shortcuts should be adapted for MacOS

## [3.8.0](3.8.0.md) (September 24, 2021)

### New features

* Create an event/metric to measure the time it takes a plugin to start (and stop)
* Implement audit records for Datasource reprocessing
* Download max file size should be configurable in admin/settings (CSV/EXCEL Plus)
* Show edit, delete, duplicate, and start-stop buttons for Integrations and Customizations search

### Fixes and improvements

* UI/UX improvements
  * \[fixed] Provider dropdown should handle a list with many items
  * \[fixed] Grouped tooltip disappears in some hover points
  * Improve `Admin>System and Display Settings>Security>Maximum concurrent sessions` description
* \[fixed] Memory leaks&#x20;
* \[fixed] Files being exported blank when using the console's download feature (CSV/EXCEL Plus)
* \[API] Live will log classloader mismatch

## [3.7.0](3.7.0.md) (September 10, 2021)

### New features

* Settings for unexpected restarts metric ([more](3.7.0.md#settings-for-unexpected-restarts-metric))
  * Available at `Admin>System and Display Settings>Monitoring`
* Add custom permission to manage standalone widgets ([more](3.7.0.md#add-custom-permission-to-manage-standalone-widgets))

### Fixes and improvements

* \[fixed] Default CSV/EXCEL download should not merge timestamps&#x20;
* \[fixed] Read-only mode should disable any interaction with Toggle
* Dashboard's performance report UI/UX improvements

## 3.6.2 (September 9, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.6.1](./#3-6-1-september-9-2021) stable version
* Update pipes to 0.25.1
  * Added `isFinite`, `isInfinite` and `isNaN`
* Fix translation (live-ui plugin)&#x20;

## 3.6.1 (September 9, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.6.0](./#3-6-0-august-27-2021) stable version
* New feature flag for disabling Timespan Boundary Adjustment, at `Admin>System and Display Settings>Pipes`
* Update pipes to 0.25.0
*
  * Added legacy operators that support division by zero: `/?`, `//?` and `%?`
  * Reduced ForkPipe's memory footprint
* UI/UX improvements
  * \[fixed] Resizing query pane in modal affects another form component in the monitoring page

## [3.6.0](3.6.0.md) (August 27, 2021)

### New features

* \[Security] Generate audit event during login using HTTP Basic Authentication
* \[API] Add option to filter lookup table values by userId

### **Fixes and improvements**

* Performance optimization for short-lived queries
* Add more information to metric events
* UI/UX improvements&#x20;
  * Setting to disable storage cache for explicit event types can now handle huge filters
  * Uniforms the icon to represent the ordering criteria of table widgets
  * Improvements in Storage stats administration page to support large collection names and better alignment in higher resolution screens
  * Configuration menu now closes when an item is selected&#x20;
  * Improve delete messages in confirmation modals
  * Improve `Admin>System and Display Settings>Log Events` description
  * \[fixed] Widget footer is blocking interactions
  * \[fixed] Actions on purge rules screen seems to be deactivated
  * Multiple changes related to the theme, e.g. the console&#x20;

## [3.5.0](3.5.0.md) (August 13, 2021)

### New features

* Added basic support for viewing & editing widgets outside dashboards
* New metric about unexpected restarts

### **Fixes and improvements**

* Dashboard Performance Report ([more](3.5.0.md#dashboard-performance-report))
  * \[fixed] If a user without permission clicks on a link on the Report, it shows an empty screen
  * \[fixed] If a user opens the Report while the dashboard is loading, it shows an error screen
  * Format long time periods using different time units (ms, s, m, etc)
* \[fixed] Export dashboard option is displayed even without the [plugin widget-export ](https://marketplace.intelie.com/artifact/plugin-widgetexport)&#x20;
  * [Plugin widget-export ](https://marketplace.intelie.com/artifact/plugin-widgetexport) 3.1.0 is required&#x20;
* \[fixed] Uniforms the icon to represent the ordering criteria of tables and lists
* Add missing English translations in confirmation modal
* Plugin admin page ([more](3.5.0.md#dashboard-performance-report))
  * Verify broken dependencies inter-plugins and show a consolidated warning&#x20;
  * Validates installed plugins and recommends download from [marketplace](https://marketplace.intelie.com/) whenever important plugins are missing
* Update the fallback page with links to download [live-ui](https://marketplace.intelie.com/artifact/live-ui/), when it's not available

## [3.4.0 (July 30, 2021)](3.4.0.md)

### New features

* Widget actions redesign
* New Extension Point to customize Logos
* New metrics about annotation creation

### **Fixes and improvements**

* Cumulative fixes from [3.3.2](./#3-3-2-july-30-2021) stable version
* Update pipes doc link to the latest version
* \[fixed] Dashboard addons should be rendered in registration order

## 3.3.2 (July 30, 2021)&#x20;

### **Fixes and improvements**

* Cumulative fixes from [3.3.1](./#3-3-1-july-20-2021) and [3.2.3](./#3-2-3-july-30-2021) stable version
* \[fixed] Form margin was inadvertently removed on [3.0.0](./#3-3-0-july-16-2021)

## 3.2.3 (July 30, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.2.2](./#3-2-2-july-20-2021) and [3.1.4](./#3-1-4-july-30-2021) stable version

## 3.1.4 (July 30, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.1.3](./#3-1-3-july-20-2021) and [3.0.6](./#3-0-6-july-30-2021) stable version
* \[fixed] SettingsObserver should be notified when a parent is deleted
* \[fixed] Dashboard yAxis metadata for temporal widget not working as expected with @meta query

## 3.0.6 (July 30, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.0.5](./#3-0-5-july-20-2021)  stable version
* \[fixed] File descriptor leak at dashboard download

## 3.3.1 (July 20, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.3.0](../#3-3-0-july-16-2021) and [3.2.2](../#3-2-2-july-20-2021) stable version

## 3.2.2 (July 20, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.2.1](../#3-2-1-july-16-2021) and [3.1.3 ](../#3-1-3-july-20-2021)stable version

## 3.1.3 (July 20, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.1.2](../#3-1-2-july-16-2021) and [3.0.5](../#3-0-5-july-20-2021) stable version

## 3.0.5 (July 20, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.0.4](../#3-0-4-july-16-2021) stable version
* \[fixed] Error on edit mode of empty widget

## [3.3.0](3.3.0.md) (July 16, 2021)

### **New features**

* Xrange time series chart
* Setting to disable storage cache for explicit event types
* \[ Operations ] Support for changing webserver properties
* Color picker now has a `Transparent` color option

### **Fixes and improvements**

* Cumulative fixes from [3.2.1](../#3-2-1-july-16-2021) stable version
* Performance optimization for short-lived queries
* Performance optimization for the dashboard span picker
* Non-following queries with errors are destroyed now
* \[fixed] Closing the warning message on the dashboard progress bar hides the progress bar forever
* \[fixed] Individual tooltip not showing the closest channel to the mouse cursor
* Update Pipes to 0.24.5
  * &#x20;Fix premature aggregation in UnionPipe and JoinPipe (partially fixed by 0.13.6)
  * Fixed string representation for empty Filters
  * Fixed round anomalies with big numbers
  * Added new copy constructor and merge operation on FilterRuntime
  * Fixed division, intDivision, and remainder by zero

## 3.2.1 (July 16, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.2.0](../#3-2-0-july-2-2021)  and [3.1.2](../#3-1-2-july-16-2021) stable version
* Add query span to `__queries` start events
* \[fixed] In some extreme cases, query statistics metrics can be discarded
* \[fixed] Dashboard yAxis properties can't be used if there's a @meta

## 3.1.2 (July 16, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.1.1](../#3-1-1-july-2-2021) and [3.0.4](../#3-0-4-july-16-2021) stable version
* \[fixed] In some cases, the default Time-Series widget isn't showing the current X Value on the Tooltip

## 3.0.4 (July 16, 2021)

### **Fixes and improvements**

* Cumulative fixes from [3.0.3 ](../#3-0-3-july-2-2021)and [2.29.11](../#2-29-11) stable version

## [3.2.0](3.2.0.md) (July 2, 2021)

### New features

* Settings' cache metrics ([more](../../developers/backend-api/settings.md#cache-metrics))
* Heavy queries monitoring&#x20;
* New CometD settings support in [live.properties](../../administration/configuration/live.properties.md) and in admin page
* WidgetQueryHandler Query Interceptors

### **Fixes and improvements**

* Cumulative fixes from [3.1.1](../#3-1-1-july-2-2021) stable version
* Multiple improvements for Non-Datasource queries
* UI/UX improvements on the Non-Time-Based Widgets Tooltip&#x20;
* Fix inconsistencies in the way timespan boundaries are handled
* \[API] TableForm render now has access to the  `rowIndex`

## 3.1.1 (July 2, 2021)

### Fixes and improvements

* Cumulative fixes from [3.1.0](../#3-1-0-june-1-2021) and [3.0.3](../#3-0-3-july-2-2021) stable version
* Fix console reducer tooltip

## 3.0.3 (July 2, 2021)

### Fixes and improvements

* Cumulative fixes from [3.0.2](../#3-0-2-may-7-2021) and  [2.29.10](../#2-29-10-july-2-2021) stable version
* Remove unnecessary alert when Opening/Closing Dashboard Filter dropdown
* Grouped Axes (chart header labels) do not update in widget resize
* Value widget font is too small
* Console download progress should be rounded to 2 decimal places
* Broken team audience monitoring widget

## [3.1.0](3.1.0.md) (June 1, 2021)

### New features

* Extremely heavy pipes queries will generate a warning for further analysis&#x20;
* Console area now has a reducer option
* New dashboard action: Performance Summary under the Options menu
* Non-Datasource queries are now independent
* All query warnings/errors are logged into  `__queries` events
* Custom Favicon configuration under Admin > Web Settings

### Fixes and improvements

* \[API] Update kotlin to 1.5.0
* \[API] Update react-spring to 9.1.1
* Prevents the merge of pipes queries when .limit storage hint is used
* Each plugin now has a list of dependencies in the Plugin area
* Better Systemd support&#x20;

## 3.0.2 (May 7, 2021)

### Fixes and Improvements

* Fix y-axis configuration on Query based Pipes charts
* Fix dashboard PDF download with wrong extension
* Fix horizontal charts bouncing when hovering
* Fix SQL error in Settings' creation at the root path

## 3.0.1 (April 22, 2021)

### Fixes and Improvements

* UX improvements on plugins sensitive data input
* Performance improvements for Settings

## [3.0.0](3.0.0.md) (April 9, 2021)

### New features

* [Improve sensitive information security](3.0.0.md#improve-sensitive-information-security)
  * All default plugins were migrated to this new API
  * \[API] Unified encryption service to secure sensitive data (passwords, connection secrets, etc)
* New chart analysis experience
  * Chart crosshair now follows smoothly the x-axis values instead of snapping to point values
  * Removed "**Find near points**" option&#x20;
* Option to display gaps on the Cartesian widget with [pipes modifiers](../../data-visualization/pipes-modifiers-on-standard-charts.md)
* [\[API\] ](../../developers/web-application/services/point-service.md)[Point Service new observable properties](../../developers/web-application/services/point-service.md)
* [\[API\] Support to partial index creation](3.0.0.md#api-support-to-partial-index-creation)
* [\[API\] Settings' Observers](3.0.0.md#settings-new-functionalities)
* [\[API\] Lookup tables can have dedicated permissions checks](../../developers/backend-api/lookup-tables.md#permissions)

### **Fixes and improvements**

* [Improve resilience against malfunctioning storage providers](3.0.0.md#improve-resilience-against-malfunctioning-storage-providers)
* \[API] Kotlin was added to the list of default dependencies provided by Live
* UI/UX improvements to the admin page
* Performance improvement in the Messenger
* [Prevents invalid indexes while creating multiple indexes concurrently for the same event type in the PostgreSQL storage provider](3.0.0.md#prevents-invalid-indexes-while-creating-multiple-indexes-concurrently-for-the-same-event-type-in-the-postgresql-storage-provider)
* Improved Download of CSV, Excel, and JSON formats
* [\[API\] Settings' API for transactions was removed](3.0.0.md#settings)

### Breaking changes and migration paths

* [PointService](../../developers/web-application/services/point-service.md) does not emit the Chart object on legacy API anymore&#x20;
  * Use the [new API](../../developers/web-application/services/point-service.md) instead
* Highcharts update from **7.0.3** to **9.0.1**
  * Check if your chart still works, use the Highchart's [Release notes](https://www.highcharts.com/blog/changelog/) as a reference if something breaks.
* \[API] Settings' API for transactions was removed
* \[API] Settings log is disabled by default&#x20;
* API changes in the Settings, related to the aforementioned Settings' Observers&#x20;
* Plugins that have invalid web services will be preemptively stopped by Live&#x20;
* Live will block the creation of queries inside queries due to a deadlock possibility
