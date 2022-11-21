# Live 2

## 2.29.11 (July 16, 2021)

### **Fixes and improvements**

* Cumulative fixes from [2.29.10](live-2.md#2-29-10-july-2-2021) stable version
* Dashboard filter selector should warning if the list is incomplete

## 2.29.10 (July 2, 2021)

### Fixes and improvements

* Cumulative fixes from [2.29.9](live-2.md#2-29-9-april-14-2021) and  [2.28.4](live-2.md#2-28-4-july-2-2021) stable version
* Filter search autocomplete does not find by partial names
* Download file extension conflict with dashboard name
* Unlogged users accessing internal URLs are not being redirected to the original URL

## 2.28.4 (July 2, 2021)

### Fixes and improvements

* Cumulative fixes from [2.27.3](live-2.md#2-27-3-july-2-2021) and [2.28.3](live-2.md#2-28-3-april-14-2021) stable version
* API Improvement for [Extensions](../developers/backend-api/extensions.md) status&#x20;
* API Improvement for Datasource status
* UI/UX improvement on the Ranking widget
* Performance improvement on the Rulealert page
* Fix crashes on the Web vitals &#x20;

## 2.27.3 (July 2, 2021)

### Fixes and improvements

* UI/UX improvements on the Filters tag component
* Fix Purge Rules with [Storage Hints](../pipes-queries/storage-hints.md)
* MongoDB Queries aren't showing fields with null value.
* Performance improvements on the Health check

## 2.29.9 (April 14, 2021)

### Fixes and Improvements:

* Security fixes

## 2.28.3 (April 14, 2021)

### Fixes and Improvements:

* Security fixes

## 2.29.8 (April 1, 2021)

### Fixes and Improvements:

* Fixes Filter selection having no effect on Dashboard and Chart editors

## 2.29.7 (Mar 30, 2021)

### Fixes and Improvements:

* Fixes widget crashing when `@meta.yAxis` ([Pipes modifiers on Pipes charts](../data-visualization/pipes-modifiers-on-standard-charts.md#meta-yaxis)) has unexpected format
* Dashboard and Chart editor filters now show Labels and Values

## 2.29.6 (Mar 24, 2021)

### Fixes and Improvements:

* Fix dashboard crash when a user with no _**Edit Pipes Widgets**_ permission would try to export a Dashboard
* Fix dashboard robustness in inactive tabs when queries get destroyed
* Improvement on SAMLv2 authentication robustness
* Data export fixes:
  * Dashboard  data and image export now have the same filename
  * Fix chart data export issue with plugin integrations
  * Improve date formatting on JSON exports

## 2.29.5 (Mar 12, 2021)

### Fixes and improvements

* Fix download error causing download files to be empty
* Improve Console download now uses POST. Fixes issues in some environments infrastructure not dealing very well with big GET requests

## 2.29.4 (Feb 26, 2021)

### Fixes and improvements

* Fix Internet Explorer 11 infinite loading on dashboards (since version 2.29.2)
* Fix Internet Explorer 11 crashing when on New Dashboard screen
* Fix Column chart leaving leftovers after query refresh
* Changing redirect strategy on login screens to improve security

## 2.29.3 (Feb 18, 2021)

### Fixes and improvements

* Add active filters to dashboard links on annotations
* Disable query start during window advance to avoid deadlocks
* Allow downloads to be executed using POST to avoid big HTTP headers
* \[fixed] Dashboard export was falling back to the browser

## 2.29.2 (Feb 9, 2021)

### Fixes

* \[fixed] Main header navigation collapsing all items&#x20;
* \[fixed] Status bar is back to absolute positioning on the Dashboard

## 2.29.1 (Feb 8, 2021)

### Fixes and improvements

* \[fixed] Potential issue when stopping or starting the same extension in a small time frame
* \[fixed] Legend toggler not showing in some charts
* \[fixed] Avoid indexing public pages in search mechanisms
* Overall web application performance improvements
* UX improvements

## 2.29.0 (Jan 26, 2021)

### New features

* Allow cache and queues directory to be different in OS
* Dynamic `manifest.json` to allow mobile customization
* Dashboard data and image export wizard
  * Added  `dashboardDownloadOptionsInterceptor` registry to dashboard service to allow customizations
* Add option 'fit to page' when exporting widgets
* New tooltip configuration in cartesian (including temporal) charts

### Fixes and improvements

* \[breaking] EventIterator API change to avoid stack overflow
* \[breaking] Create new authority annotation `@SkipDefaultAuthorityCheck`
* \[fixed] Potential deadlock when accessing system settings
* \[fixed] Calendars showing invalid dates
* \[fixed] Reject storage providers with the same name
* Throttle events on datasource reprocess
* UI fixes and improvements
* Performance improvements on dashboards

## 2.28.2 (Oct 7, 2020)

### Fixes and improvements

* \[fixed] Frontend query interceptors stopped working, introduced in 2.28.1
* \[fixed] Widget errors on editors are now recoverable by query refreshes&#x20;
* \[fixed] Hiding a fullscreen widget kept an overlay on the dashboard
* \[fixed] Event count estimate was susceptible to a numeric overflow
* Dark mode improvements
* UX improvements

## 2.28.1 (Sep 23, 2020)

### Fixes and improvements

* \[fixed] Frontend issue when logarithmic scales extremes are set to 0
* Default reducers are now automatically applied in charts where a custom reducer is empty or null
* Support for gaps in the console chart
* UX improvements

## 2.28.0 (Aug 21, 2020)

### New features

* Allow marking data gaps on charts using the flags `enableDataGapsReport` and `dataGapsReportThreshold` ([more](../data-visualization/pipes-modifiers-on-standard-charts.md))
* New widget selector ([more](../developers/web-application/dashboard-and-widgets/custom-widget-editors.md))
* API to allow throttled inserts and updates to the storage providers, so that operation is blocked until the persistent queue is not full (more soon)
* New web application metrics (more soon)

### Fixes and improvements

* Performance improvements on widget editor UI
* Normalize file names for data download on charts
* Allow toggling rules activity on the rule list

## 2.27.1 (Aug 12, 2020)

### Fixes and improvements

* \[fixed] Dashboard would intermittently freeze due to full bayeux queues
* Support for legacy widgets missing destroy function
* Delegate DoS filter to infrastructure

## 2.26.3 (Aug 12, 2020)

### Fixes and improvements

* \[fixed] Dashboard would intermittently freeze due to full bayeux queues
* \[fixed] Support to nested timestamp fields
* \[fixed] Fix "order by" usage in SQL as Storage
* \[fixed] Pie Chart does not display correct percent value when used together with query meta \_\_formatted
* Security improvements to TCP input integration
* Better support for timezone when exporting data as files
* UX improvements to charts

## 2.27.0 (Jul 2, 2020)

### New features

* Perform count in the background for storage queries, making load faster
* Add allowed and denied origin IPs on TCP input integration ([more](../featured-plugins/tcp-input-1.md))
* Create `starting` status to extensions such as plugins
* Added read-only permissions to all configurations
* Added `StorageLayout`to the API
* More options to customize query download via API

### Fixes and improvements

* \[fixed] Support to nested timestamp fields
* Security improvements to TCP input integration
* UX improvements to charts

## 2.26.2 (Jun 3, 2020)

### Fixes and improvements

* Allow zooming on cartesian widgets
* Sending an alert email to users after many wrong login attempts
* Better handling malformed data on TCP input integrations
* \[fixed] Wrong aggregations on all `every current` windows

## 2.26.1 (May 19, 2020)

### Fixes and improvements

* Avoid creating remember-me token for basic auth
* Show more content on rules list
* \[fixed] Dashboard not loading when widget implementation is not present
* \[fixed] Issues to send emails

## 2.26.0 (Apr 30, 2020)

### New features

* Display summary of data growth rate on storage statistics
* Memory consumption calculation on the queries administration page
* Show own permissions to current user on account information
* Allow downloading many plugins at once
* Allow customization of dashboard's timeline query
* Log lines generate `__log` events for warn and error levels
* Web analytics on event type `webhit`
* Added metrics for SQL connection pool and hibernate
  * Database: connection count, successful transaction count, rollback count, second level cache hit, miss & put count, prepare statements count, close statements count.
  * Query: cache hit, miss & put count, max query execution time, query exec count.
  * Session: open, close, flushes, entity load count, entity update count, entity insert count, entity fetch count, entity delete count, optimistic failure count.
  * Pool: available count, busy count, idle count, leak count.
* Immutable events structure \[breaking]: this makes Live faster, but requires that plugins are re-built

### Fixes and improvements

* 3x performance improvements on storage queries
* Updated mysql connector to version 8.0.18
* Permission schema improvements
* UX improvements on dashboards and widgets
* CSRF token automatic renewal
* \[fixed] HTTP security issues
* \[fixed] Incorrect memory estimation on the queries administration page
* \[fixed] Incorrect file system usage on storage statistics
* \[fixed] Possibility of out of memory error when async queries were enabled

## 2.25.19 (Apr 7, 2020)

### Fixes and improvements

* \[fixed] Temporal charts losing the context and stopped plotting data
* \[fixed] Blank dashboards on IE
* \[fixed] Security issue in browser's local storage usage by messenger

## 2.25.18 (Apr 2, 2020)

### New features

* Configurable limit of the number of form submission requests per second
* Configurable limit of concurrent logins for the same user (default to unlimited)
* Entity change listeners
* Allow to include prefix on email subjects

## 2.25.17 (Mar 20, 2020)

### Fixes and improvements

* \[fixed] Session ID not updated after user login
* \[fixed] Performance issues on heatmap chart
* Added support to submit-and-close button on forms
* UX improvements on application header

## 2.25.16 (Mar 4, 2020)

### Fixes and improvements

* \[fixed] Dashboard JSON import ignoring span picker type&#x20;
* \[fixed] Chart X axis not using the configured date format

## 2.25.15 (Feb 27, 2020)

### Fixes and improvements

* \[fixed] Updated Jetty to fix a vulnerability issue
* Improvements on widget PDF/image export process

## 2.25.14 (Feb 18, 2020)

### Fixes and improvements

* \[fixed] Purge rules did not show on administration
* Increased the timeout that server waits for the client on cometd transport layer to avoid unwanted query reloads
* Increased maximum message size on cometd transport layer to avoid discarding big messages

## 2.25.13 (Feb 11, 2020)

### Fixes and improvements

* \[fixed] Settings version logs would show no author
* Performance improvements on charts rendering

## 2.25.12 (Feb 5, 2020)

### Fixes and improvements

* \[fixed] Reversed min and max values on the temporal chart axis
* \[fixed] Previous queries on widget editor overriding current results
* Added support to `pipes.group` on meta parameters ([more](../pipes-queries/meta-parameters.md))

## 2.25.11 (Jan 30, 2020)

### Fixes and improvements

* Allow different users to save personal settings on the same browser window

## 2.25.10 (Jan 27, 2020)

### Fixes and improvements

* \[fixed] Potential deadlock during simultaneous accesses to the settings API

## 2.25.9 (Jan 24, 2020)

### Fixes and improvements

* \[fixed] Performance issues due to excessive accesses to the configuration database

## 2.25.8 (Jan 15, 2020)

### New features

* Allow plugins to add menu options to dashboards

### Fixes and improvements

* \[fixed] Condition that should freeze the dashboard in the loading state
* \[fixed] Condition that makes temporal widgets plot data out of order
* \[fixed] Issues on the dashboard's calendar span picker
* Allow choosing the filename when sending an email directly from a widget

## 2.25.7 (Dec 17, 2019)

### New features

* Heatmap widget ([more](../data-visualization/pipes-widgets/heatmap-widget.md))

### Fixes and improvements

* Allow the creation of annotations on any messenger room
* Improvements on charts tooltips rendering
* \[fixed] Charts Y axis display errors

## 2.25.6 (Dec 9, 2019)

### Fixes and improvements

* UX improvements on the dashboard's time span selector
* Allow users to share widgets via email
* \[fixed] Public dashboard header layout alignment issue
* \[fixed] Grouped axes labels alignment issue

## 2.25.5 (Nov 22, 2019)

### Fixes and improvements

* Display Pipes metadata information for console queries
* Allow changing the span and the filters during the dashboard load
* \[fixed] Charts tooltip formatting errors
* \[fixed] Legends not clickable in some situations
* \[fixed] Typing on console during a query execution makes cursor go to the beginning of the line

## 2.25.4 (Nov 8, 2019)

### Fixes and improvements

* Keep series hidden after reloading a dashboard
* Allow rendering HTML content on web alerts
* Allow a dashboard to be locked for edition
* Do not show "too many queries" alert for deliberate types on queries
* General UX improvements

## 2.25.3 (Oct 24, 2019)

### Fixes and improvements

* Allow exporting charts in A4 (portrait and landscape, with paging) formats in PDF
* \[fixed] Concurrency issue that could make events be deleted instead of updated
* \[fixed] Error duplicating an existing dashboard on the wizard

## 2.25.2 (Oct 3, 2019)

### Fixes and improvements

* \[fixed] Concurrency issue that could lead to a deadlock after a plugin update
* \[fixed] Some internal control events could be lost during Live startup
* Improvements on widgets tooltips

## 2.25.1 (Sep 17, 2019)

### Fixes and improvements

* \[fixed] Widgets resize not working properly on dashboards

## 2.25.0 (Sep 16, 2019)

### New features

* Support SQL integrations as configuration databases

### Fixes and improvements

* Replace FST with [Disq](https://github.com/intelie/disq) serialization format in cache segments
* MongoDB write performance improvements
* MongoDB query performance improvements
* Persistent queue performance improvements
* Delay query initialization during Live and plugin startup
* TCP input performance improvements
* \[fixed] Queries causing RuntimeException: Method code too large

{% hint style="warning" %}
From this version on, Live initialization will wait for all the plugins to be fully available. Then, it is not possible to perform synchronous queries on any plugin startup. There is a timeout of 3 minutes to avoid that such situations prevents Live from starting.
{% endhint %}

## 2.24.5 (Aug 16, 2019)

### Fixes and improvements

* Improvements on web application bundles sizes

## 2.24.4 (Aug 5, 2019)

### Fixes and improvements

* \[fixed] Issues on widget PDF and PNG export

## 2.24.3 (Jul 26, 2019)

### Fixes and improvements

* Changes in dashboard url are now added to browser history
* \[fixed] Dashboard making duplicate requests

## 2.24.2 (Jul 18, 2019)

### Fixes and improvements <a href="#fixes-and-improvements" id="fixes-and-improvements"></a>

* Improvements to widget PDF and PNG export
* Improvements to legacy browsers support
* \[fixed] Possible query leak on messenger

## 2.24.1 (Jul 4, 2019)

### Fixes and improvements <a href="#fixes-and-improvements" id="fixes-and-improvements"></a>

* Make optional the refresh of the queries after overwrites on the storage or after events that skip the real time engine
* \[fixed] Error during dashboard duplication

## 2.24.0 (Jun 21, 2019)

### New features

* Download API
* Query API refactoring
  * Create context and parameters in Query object, mapping to `@@context` and `@@params` macros
  * Move fixDescription and add url, host, user from Query to context
  * Move query-related API functions from Live.Engine to a new Live.Query interface, deprecate old ones
  * Enrich queries with context about who, how and why a query was executed, including
    * user timezone
    * which user started the query
    * query source (datasource, rule, widget, alert, etc.)
    * which plugin asked for it
* Allow custom parameters on queries
* Query statistics page
* Add hint flag to force automatic index creation
* Save last login date for each user
* Generate events for web usage statistics
* Allow setting session timeout in the Live interface
* Allow choosing the authentication provider for a specific user

### Fixes and improvements

* Improvements to storage statistics interface
* Allow WidgetQueryHandle to know whether a query was started by a download
* \[fixed] Auth provider NPE causes failure on start
