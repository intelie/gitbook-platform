# Entity Audit

{% hint style="info" %} Since Live @ 3.33.0 {% endhint %}

**Intelie Live** provides a mechanism to enable auditing writing operations with Entities as database records or as events. This feature is not mandatory and the admin need to turn it in the **General Settings** area on in the **SYSTEM AND DISPLAY SETTINGS** menu.

{% hint style="warning" %} Currently, Live does not provide any purge mechaninsm for the entity audit stored in the database. Over time, this can affect the database performance. {% endhint %}

After enabled, **Live** will store the writing operations as records of the `EntityAudit` table in the database and/or as events of __type__ `__audit` and __src__ `core`.