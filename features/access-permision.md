# Access Permission

## Permission schema

The access level in Intelie Live can be restricted by features (e.g. Access to Console), by entity types (e.g. reading or writing dashboards, rules, and so on), and by content (entity instances).

![Permission model in Intelie Live](<../.gitbook/assets/image (75).png>)

Permissions restrict access to functionalities. They are defined at development time by the plugins or by Intelie Live itself.

Roles can be created on the web administration interface and contain sets of permissions. Those roles can be related to teams, which also contains perspectives and users.

Perspectives are groups that allow access to entities, respecting the access level defined by the permissions.

![Creating a new role in Administration](<../.gitbook/assets/image (77).png>)

## User Basic Permissions

Some of the permissions are provided by default by the Intelie Live platform. Plugins can create any permission in Intelie Live, and test for them before providing access to features that they provide.

{% hint style="warning" %}
A user with `Manage system` permission will have the permission to view and edit any entity or feature. A user with `Console` permissin will be able to see all the data in the system.
{% endhint %}

![Depending on user permissions, more features can appear in this menu](<../.gitbook/assets/image (117).png>)

| Permission                                                                                                                                                                                              |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Console Access`                                                                                                                                                                                        |
| Access to the query console (available at `/#console`). This permission grants to any user access to all data available at Intelie Live.                                                                |
| `Dashboard Access`                                                                                                                                                                                      |
| Grants access to view dashboards that are in the perspectives that the user has access.                                                                                                                 |
| `Manage Dashboards`                                                                                                                                                                                     |
| Grants access to edit dashboards that are in the perspectives that the user has access. This does not grant to user the permission to edit Pipes widgets, then it does not imply in access to all data. |
| `Manage Pipes Widgets`                                                                                                                                                                                  |
|  Grants to the user the right of editing Pipes widgets. This permission grants to any user access to all data available at Intelie Live.                                                                |
| `View high frequency data configuration`                                                                                                                                                                |
| Grants to the user view access to configuration of high frequency data related features.                                                                                                                |
| `Manage high frequency data configuration`                                                                                                                                                              |
| Grants to the user manage access to configuration of high frequency data related features.                                                                                                              |
| `View static data configuration`                                                                                                                                                                        |
| Grants to the user view access to configuration of static data related features.                                                                                                                        |
| `Manage static data configuration`                                                                                                                                                                      |
| Grants to the user manage access to configuration of static data related features.                                                                                                                      |
| `View permission configurations`                                                                                                                                                                        |
| Grants to the user view access to configuration of permission related features.                                                                                                                         |
| `Manage permission configurations`                                                                                                                                                                      |
| Grants to the user manage access to configuration of permission related features.                                                                                                                       |
| `Manage configurations`                                                                                                                                                                                 |
| Grants to the user the right of editing Datasources, Perspectives, Streams and Lookup Tables.                                                                                                           |
| `Manage system`                                                                                                                                                                                         |
| Grants access to the administration section. This permission grants to the user access to all data and the right to change any configuration on the system.                                             |
| `Create/Edit annotations`                                                                                                                                                                               |
| Grants to the user permissions to add annotations on the timeline of any dashboard and on the messenger.                                                                                                |
| `View messenger chat rooms`                                                                                                                                                                             |
| Grants to the user the permission to use the messenger.                                                                                                                                                 |
| `Create messenger chat rooms`                                                                                                                                                                           |
| Grants to the user the permission to create rooms in the messenger. It implies on granting access to the messenger.                                                                                     |
