# Introduction

## Service providers

The Intelie Live platform classifies its extenstions in the following six different **roles**. Generally, there are many simultaneous instances of those extensions running.

### Query providers

Query providers serve as data processors for both real-time and historical data. A query provider can be either a remote provider or a local provider. When a remote provider is used, it usually generates new data for Intelie LIVE (e.g. using a database integration as a query provider allows to query this database directly from the web interface). Local providers assume data is received and process it.

Common examples for remote providers are Google Analytics and Twitter, and for local providers Pipes and Esper.

### Storage providers

Storage providers add to the platform the ability to connect to remote systems to persist data. This data can be used by Intelie Live itself or by other systems. Intelie Live can use one or many integrations as databases at a time. Those systems must be NoSQL or schema-less databases. Common integrations are MongoDB, MariaDB and PostgreSQL.

{% hint style="info" %}
It is important to note that Live does not require an enabled storage integration to work in real time.
{% endhint %}

{% hint style="info" %}
Besides the storage system, Intelie LIVE implements a persistent queue and a cache on the local disk.
{% endhint %}

### Data input providers

Input providers add to the platform the capacity of receiving data in different protocols and formats.  These integrations can be both on servers, which will wait for data to come, or clients, which will request data.

Common examples of input providers are TCP Input and REST Input.

### Data output providers

Output providers add to the platform the ability to either write data actively on remote systems or start a local server to receive requests. Common examples of this are REST Output and many industry-specific simulators.

### Authentication providers

Authentication providers may act as authenticators for user accounts. Common examples are database authentication, LDAP, Active Directory and Oauth2.0.

### Notification providers

Notification providers can be triggered in specific situations, or be called directly by plugins (or even from the web interface). Common examples are email (IMAP and SMTP), Amazon SNS, ActiveMQ and Twitter.

{% hint style="info" %}
Any extension can define and enable one or more roles at once.
{% endhint %}
