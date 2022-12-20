---
description: The plugin purge delivers a new design for the Live Purge subsystem
---

# Purge Plugin

The plugin purge delivers a new design for the Live Purge subsystem. It improves the purge performance using batch operations supported by Live Storage Providers.. 

This document provides an overview of the new purge plugin and the features of the recently released 1.1.0 version. In addition, it compares the new plugin with the Live purge subsystem and describes the features planned for the upcoming version.  

Even though the purge plugin does not yet cover all features of the Live Purge subsystem, it improved the purge performance. In local tests, the purge executed three times faster using the new plugin compared with the current subsystem. There is no plan to remove the Live Purge subsystem, so it can still be used. 

# Features
## Configuring rules

After installing the plugin, two new options will be available on the admin menu: **Purge** and **Purge History**. The rules are available on the Purge page.

![New purge subsystem administration menu](https://user-images.githubusercontent.com/71188443/207718319-dcd1f422-3c4e-40b1-8c62-dc861d3311f5.png)

The first time accessing this page, no rules will be configured and the following message will be shown. You just need to click on the button to create a new rule.

![Create new purge rules](https://user-images.githubusercontent.com/71188443/207718398-049a73c3-9bd2-4650-9e32-11b206ce070d.png)

A rule has three configurations:
- **Event type**: the type which will be purged;
- **Time range**: the span or time interval to be purged, e.g., from ts 0, until last month, 2022-11-21 00:00 to 2022-11-30 23:59;
- **Recurrence**: a rule can be executed only manually on this page (one-time), or it can be scheduled (recurrent) to execute both manually and automatically based on the configured cron, which we will cover later in this document.

![New purge rule configuration](https://user-images.githubusercontent.com/71188443/207718483-7b3e5d9f-2735-4e2c-be4d-79a62f432dac.png)

After saving a rule, a list of rules will appear. In this list, a rule can be edited or deleted.

![Purge rules listing](https://user-images.githubusercontent.com/71188443/207718563-3db482a8-1c4e-4777-83de-8012e142d319.png)

Once at least one rule is configured, two new buttons will be available:
- **Simulate**: runs a simulation to estimate how many events would be deleted considering the configured rules;
- **Run purge**: runs a real purge, deleting events based on the configured rules.

![User actions of purge subsystem](https://user-images.githubusercontent.com/71188443/207718589-bdaac8bc-0b2b-4324-816f-2a226ea52ff5.png)

When a **Simulation** or **Purge** is running, a cancel button replaces the **Simulate** and **Run purge** buttons and can be used to cancel the current task. The status of the current execution and a progress bar will be displayed. The progress bar shows the progress regarding the number of purge rules executed.

![Progress of the purge execution](https://user-images.githubusercontent.com/71188443/207718657-7f3d2ad5-42c6-4caa-a359-05d2cf643536.png)

There is also a “show more” button, which will show more details about the current task execution.

![Purge details dialog](https://user-images.githubusercontent.com/71188443/207718818-637658e8-6c01-4e38-bb4f-3f8be1c42a12.png)

## Checking previous executions

It’s possible to check previous purge executions on the **Purge History** page.

![Purge history administration page](https://user-images.githubusercontent.com/71188443/207718909-91362715-7c9c-487a-a6df-e851dc357f02.png)

A list of executions and their information are displayed, along with the “show more” button, which will show more details.

## Purge status
The status of a purge execution is displayed on the Purge History page, but it can also be checked using the ```__plugin_purge``` event type. A purge execution can have five status:
-	**Waiting**: only four simulations can run in parallel, or one “real” purge at a time. Other executions will be waiting when these limits are reached.
-	**Running**: the purge execution is running.
-	**Completed**: the purge execution finished successfully.
-	**Canceled**: the purge execution was canceled, by the user or by the auto-cancelation feature.
-	**Failed**: some error caused the purge execution to fail.

# Purge system settings

All settings of the purge plugin are located on the **System and Display Settings** administration page.

![Purge configuration card on System and Display Settings](https://user-images.githubusercontent.com/71188443/207719501-315c3b5c-512a-4b68-95d4-ab77655acd7d.png)

## Maximum duration configuration
It’s possible to configure a maximum duration for a purge execution. When the duration is exceeded, the task will be automatically canceled.

![Maximum duration configuration](https://user-images.githubusercontent.com/71188443/207719557-b09467ae-a8d2-42e6-b4cb-4d0a85a2fc10.png)

## Scheduled rules configuration

The scheduled execution of purge rules can be enabled or disabled with the first toggle button. Then there’s a Cron configuration, which has its own syntax to period of time (https://crontab.guru/). If the scheduled execution is enabled, all purge rules configured as Recurrent will be executed based on this periodicity. In the example below, all recurrent rules would be executed every day at 10:55AM.

![Scheduling the recurrence of purge rules execution](https://user-images.githubusercontent.com/71188443/207719608-38edfd20-2d1c-472d-933a-0efd582e4ec0.png)

# Comparing the new plugin against the classic Live purge system

The classic Live Purge system is deprecated in Live v3 series and it's expected to be removed on Live v4 in favor of the Purge plugin.

## Simulation performance
A collection with approximately 11.000.000 events was created and then simulations using the new plugin and the current system were executed. It took 25 seconds on the new  plugin, and 59 seconds on the current system.

![New purge plugin](https://user-images.githubusercontent.com/71188443/207719774-5e63f8e7-5ad1-47cf-9f42-27448c83fc8a.png)
![Classic Live core purge subsystem](https://user-images.githubusercontent.com/71188443/207719790-ccba3761-0150-43a9-89d2-0440bc232027.png)

## Purge performance

The same collection with 11.000.000 events was used to run a real purge, deleting the events using both strategies. It took 2 minutes and 30 seconds using the new plugin, and 6 minutes and 22 seconds using the classic system.

![New purge plugin](https://user-images.githubusercontent.com/71188443/207719914-2425c301-d694-4406-9f4b-09d6da56063a.png)
![Classic Live core purge subsystem](https://user-images.githubusercontent.com/71188443/207719936-4bf7da61-1507-4242-968e-153ba6d22e83.png)

## Features comparison

-	The classic Live purge subsystem:
    - **does not** allow the user to schedule executions (it is only possible using a groovy script) 
    - **does not** delivery a built-in maximum duration (again, only possible using a groovy script)
    - **uses** a generic pipes filter instead of event type (more powerful in terms of configurability but poor in performance)
    - **allows** the configuration of exception filters to prevent some events from being deleted

## Planned features for upcoming version

### Support to wildcards (1.2.0)
At the moment (version 1.1.0), the event type of a purge rule only accepts a string representing a real event type. We are working on the wildcard support, making it possible to delete more than one event type on one rule.

### Keep rules (1.2.0)
The classic purge system at Live core has the exception concept, to prevent event types from being deleted. We will implement keep rules to substitute this feature, making it more intuitive. This feature will be useful when used together with wildcard on delete rules.

### Space freed estimation (1.2.0)
The classic purge system has information about how much space is being freed when running a purge task. We plan to add this feature to our plugin.

### Cancel button on Purge History (1.2.0)
Currently it’s only possible to stop a purge task on the Purge page, but once a user leaves, it’s not possible to stop it. A cancel button will be added on the Purge History page to let the user stop running purge tasks.

### Script to migrate rules (1.2.0)
A script to migrate rules from the classic purge system to our plugin is being developed.

# Limitations

-	It is not possible to estimate how much time a purge execution will take to finish based on its rules.
-	The ```__plugin_purge``` events are emitted after each rule is executed, so the user will not notice any changes on the interface until a rule is finished. If a rule takes too long, the progress will also take this time to be updated.
-	The purge task can’t be canceled if a rule is already being executed, it needs to wait until the current rule execution is finished and then the process is canceled. If a rule takes too long, the cancel will not be noticed immediately.
