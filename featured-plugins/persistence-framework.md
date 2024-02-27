---
description: The plugin Static Data Persistence Framework offers a new possibility to persist static data
---

# Static Data Persistence Framework

## About

The main objective of the Static Data Persistence Framework (SDPF) is to facilitate the creation of contracts related to reading and writing static data, through JSON schemes that represent the business logic of a specific domain.

In just a few steps, the developer has access to the static data management APIs.
Simply map the data models into JSON schemas, register them in the application and, using the generic APIs available through a LIVE service, it is possible to perform static data CRUD.

## Architecture

The Static Data Persistence Framework is divided into 4 blocks, where each of these blocks has a specific functionality for the plugin to work.

 - **The persistence framework block**: Its objective is to provide the endpoints that the static data application needs to perform data CRUD.
It also provides, through Intelie Live settings, access to Json Schemas management. These schemas will be stored in the system to link them to data in the future.


- **Apicurio block**: Manages json schemas.
The function of this layer is to act as a facilitator for the storage and use of the schemas necessary for the correct functioning of the application.


- **The spark server**: It operates as a middleware between persistence and the parquet database.
It provides endpoints for manipulating data related to a given schema.
Contains the application's data processing logic and is responsible for managing relationships between entities.


- **The Parquet Database**: Layer where data from the application is stored in different partitions.
In the current modeling, each dataset is saved as a different partition in the parquet database.

![SDPF Architecture](../.gitbook/assets/image%20(190).png)
<p style="text-align: center; font-style: italic">Image 1: SDPF Architecture</p>

## Features

In version 1.0.0, we have the following features for SDPF:

- Domain schema modeling

- Static data management through generic Intelie Live services (APIs), as:
  - Data insertion
  - Data update
  - Data removal
  - Data querying


- Management of simple data relationships, through generic Intelie Live services (APIs), as::
  - Relationship insertion
  - Relationship removal
  - Relationship querying

## Environment Configuration

For the Static Data Persistence Framework to work correctly, in addition to installing the plugin, it's necessary to have two services running, in addition to Intelie Live:

- Spark Server
- Apicurio

For more details on how to configure an environment with SDPF, you can find this [configuration documentation](https://viasatinc.sharepoint.com/:w:/r/sites/intelie/Intelie%20Platform/Teams/Data%20Fusion/Products/Static%20Data%20Persistence%20Framework%20(SDPF)/Static%20Data%20Persistence%20Framework%20-%20(Local)%20Configura%C3%A7%C3%A3o.docx?d=w8f0bf4c95ab14f7dbac151fcae7e9547&csf=1&web=1&e=tSAdeL).

## Modeling schemas, schema subsets and relationships

### Modeling schemas

Currently, SDPF works with a single type of Json Schema, which is called a modeling schema.

This type of schema serves the basic modeling of the data to be managed, functioning in a similar way to a relational database schema. In other words, it represents the logical configuration of the entire database related to a given entity.

Each of the fields in a modeling schema defines the set of rules that a given attribute of an entity must meet in order to be persisted.

To save a new modeling schema in SDPF, you need to go to the Intelie Live administration area, in the “Persistence” Menu.

![Modeling schema insertion](../.gitbook/assets/image%20(191).png)
<p style="text-align: center; font-style: italic">Image 2: Saving a new JSON Schema in SDPF</p>

The model to be saved must meet the criteria of a json schema. Some examples can be found on this [website](https://json-schema.org/learn/miscellaneous-examples).

### Subsets of a modeling schema

A subset of a modeling schema, as the name suggests, is a set of data derived from a modeling schema.

It is not necessary to register subsets in the SDPF. This concept only serves to represent, in an abstract way, a part of the data that you want to search for.

In other words, a subset of a modeling schema works as a projection, making it possible to search for data in a partial way, where only certain fields are returned, as long as they belong to a previously persisted modeling schema.

### Related schemas

Data can be related to each other.

In SDPF, the relationship between entities is represented by a schema internal to the application, which saves information about the datasets related to each entity.

For now, this relationship schema is immutable and transparent to the user, but this may change in the future.

Relationship modeling in SDPF was designed analogously to a nxn relationship table.
This way, it is not necessary to save information about the relationship between entities within the entity itself, allowing relationships to be updated independently, without any update to the data already persisted for the entities.

### View Schemas

View schema is a concept created to represent data searches within SDPF.

Instead of defining a set of rules that certain data must meet, as in the modeling schema, the visualization scheme defines, in a structural way, how a piece of data must be returned, once a query is made for it.

We can create a visualization schema that acts as a subset of a modeling schema, in order to partially return data, depending on the application's use.
Or, we can define a visualization schema that, instead of returning data individually, returns data containing an aggregation of their respective relationships, or just some fields of their relationships.

**PS: This feature is not yet available in SDPF 1.0.0.**

## How to use generic APIs for data CRUD

### Use through Plugin Service

After installing and configuring the Static Data Persistence Framework, simply import the StaticDataService class into the application you want to use and make use of the public APIs.

Some examples can be found in the [technical documentation](https://viasatinc.sharepoint.com/:w:/r/sites/intelie/Intelie%20Platform/Teams/Data%20Fusion/Products/Static%20Data%20Persistence%20Framework%20(SDPF)/Static%20Data%20Persistence%20Framework%20Doc.docx?d=w9623eb7421b944178cedd9e573dfb59e&csf=1&web=1&e=fTNKfB).

### Use through REST APIs

Public APIs for static data management are also available for use through REST APIs.

Requests must be made to an Intelie Live service, prefixed with “http://[LIVE_HOST]:[LIVE_PORT]/services/plugin-persistence-framework/static_data”.

Some available endpoints can be found in the [technical documentation](https://viasatinc.sharepoint.com/:w:/r/sites/intelie/Intelie%20Platform/Teams/Data%20Fusion/Products/Static%20Data%20Persistence%20Framework%20(SDPF)/Static%20Data%20Persistence%20Framework%20Doc.docx?d=w9623eb7421b944178cedd9e573dfb59e&csf=1&web=1&e=fTNKfB).

## Schemas API

### Use through Plugin Service

To register modeling schemas in SDPF, it's recommended to use the interface available with the plugin.

However, it's also possible to manage schemas by importing the ApicurioService class into the application you want to use and making use of the public APIs.

Some examples can be found [here](https://viasatinc.sharepoint.com/:w:/r/sites/intelie/Intelie%20Platform/Teams/Data%20Fusion/Products/Static%20Data%20Persistence%20Framework%20(SDPF)/Static%20Data%20Persistence%20Framework%20Doc.docx?d=w9623eb7421b944178cedd9e573dfb59e&csf=1&web=1&e=fTNKfB).

### Use through REST APIs

It's also possible to manage schemas in SDPF through REST requests to public APIs.

Requests must be made to an Intelie Live service, prefixed with “http://[LIVE_HOST]:[LIVE_PORT]/services/plugin-persistence-framework/schemas”.

Please visit the [docs](https://viasatinc.sharepoint.com/:w:/r/sites/intelie/Intelie%20Platform/Teams/Data%20Fusion/Products/Static%20Data%20Persistence%20Framework%20(SDPF)/Static%20Data%20Persistence%20Framework%20Doc.docx?d=w9623eb7421b944178cedd9e573dfb59e&csf=1&web=1&e=fTNKfB) to find some examples.

### Use through Apicurio Schema Registry

It's also possible to manage schemas internally in the Apicurio service, through the Schema Registry.

**We do not recommend** this approach, given that any schema change made outside the expected SDPF cycle may compromise the integrity of the persisted data, since all data is linked to json schemas managed by Apicurio.

There is the intention, in future versions of SDPF, to implement schema versioning to circumvent this need.

### Note on Schema Management

Caution is recommended when managing modeling schemas that already have data persisted, even through the forms provided by the plugin.

Currently, SDPF does not make any kind of restrictions on removing/updating modeling schemas. An inadvertent update can result in undesired behavior and compromise the integrity of persisted data, as all data within the application is tied to a certain modeling schema.

There are plans to improve schema management in the future, including versioning, aiming to overcome this detail and make the update clearer and more objective.

For now, caution is recommended.

## Version Limitations

In SDPF 1.0.0, there are some known usage limitations, which will be improved in future versions:

### Using View Schemas

Although it's possible to determine which fields will be projected in a data query, it's not possible to aggregate multiple schemas and return a new set of data that encompasses the attributes of those schemas.

### Removing and updating schemas through the UI

Although we provide schema removal/updates through public APIs, this usage is not reflected in the interface.

Removing a schema without any treatment of the persisted data can generate unwanted inconsistencies. There are several possibilities to address this problem, but it is necessary to better understand the plugin's use cases in order to apply the most comprehensive approach.

### Creating relationships for several different entities

SDPF allows the creation of relationships from an entity A to an entity B, n x n. However, once a relationship is created from A to B, a relationship to another entity C is not expected to be created from A.

Current modeling allows this creation, but the data search logic does not yet consider this use case. There are improvement plans in future releases.

### Apicurio Registry version limitations

To maintain compatibility with Intelie Live 3.x, the latest version of Apicurio Registry compatible with SDPF 1.0.0 is 1.3.2.Final, which in turn requires a postgres database up to version 12.