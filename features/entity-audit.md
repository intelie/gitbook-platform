# Entity Audit

{% hint style="info" %} Since Live @ 3.33.0 {% endhint %}

**Intelie Live** provides a mechanism to enable auditing write operations on **Live** entities as database records or as events. This feature is disabled by default, and can be enabled in the **General Settings** page, in the **SYSTEM AND DISPLAY SETTINGS** menu.

After enabled, **Live** will store the writing operations as records of the `EntityAudit` table in the database, and/or as events of __type__ `__audit` and __src__ `core`.
