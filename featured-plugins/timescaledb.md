---
description: TimescaleDB expands PostgreSQL for time series and analytics.
---

# TimescaleDB

## TimescaleDB Plugin

TimescaleDB plugin is built over PostgreSQL and handles relentless streams of time-series data with the performance, scalability, and usability that the application needs.

This document provides an overview of the new timescaledb plugin and the features of the recently released 1.5.0 version.
In addition, it describes the features planned for the upcoming version.

TimescaleDB plugin was designed with two main goals: Improve query performance and reduce storage costs, when compared to other Intelie Live Plugins.
This document will also cover some comparison among them.

## Data Layout

This plugin stores Live events organized by event type in separated hypertables.
Each hypertable can have a custom a timestamp column to be the one that will be used at chunk compression level, and the administrator can pre-configure those mappings.

![TimescaleDB time column configuration](<../.gitbook/assets/image (177).png>)

## Core concepts

### Hypertables

Hypertables are PostgreSQL tables with special features that make it easy to handle time-series data.
Anything you can do with a regular PostgreSQL table, you can do with a hypertable.
In addition, you get the benefits of improved performance and user experience for time-series data.

### Time column

Time series data is partitioned inside hypertables on a time parameter, called "time colum".
The time column is usually a field from the Live event which stores time information, which can have different meanings.
A time field is typically mapped to be the hypertable's time column.

### Chunks

Behind the scenes, the database performs the work of setting up and maintaining the hypertable's partitions.
Those partitions are called "chunks", which is the smallest portion of data that can be compressed and decompressed.
The chunks can be considered many child tables, which compose the hypertable.

![Hypertable chunks](<../.gitbook/assets/image (178).png>)

### Compression

Compression reduces the amount of space taken up by your data. For some queries, it also speeds up query time, because fewer bytes need to be read from disk.
TimescaleDB uses native compression for hypertable data. That means it doesn't need a specific file system or extra software. Compression works out-of-the-box.
Compressed data is stored in a hybrid row-columnar format. This format works best for older data, which is queried but rarely changed.
Newer, often-changed data should be stored in uncompressed row format.

- Uncompressed Chunk:

![Uncompressed hypertable](<../.gitbook/assets/image (179).png>)

- Compressed Chunk:

![Compressed hypertable](<../.gitbook/assets/image (180).png>)

### Segmentation

When we compress data, we might choose a column on which the data will be segmented.
We can think of segment by column as the way data is grouped when compressed in TimescaleDB.

In other words, each row in a compressed table must contain data about a single item.
The column that a table is segmented by contains only a single entry, while all other columns can have multiple arrayed entries.
For example, if a hypertable is segmented by decide ID, this is how the data will be arranged after compressed:

![Segmented hypertable](<../.gitbook/assets/image (181).png>)

Unlike above, each compressed row has information about a single device ID.
This can be particularly efficient on queries that use the segmentation column in the WHERE clause, because the decompression can happen after filtering.

It's important to note that segmentation columns are not compressed.
Instead, for each value of the segmentation column, a separate compressed row is created.

## Hypertable's Management

With TimescaleDB plugin, the event types can be managed in a specific tab.
This is pretty much like the event type management in other Live plugins.
The difference is that each event type is mapped to a hypertable, and come additional features are included:

![Hypertable Management](<../.gitbook/assets/image (182).png>)

## Index Creation

TimescaleDB plugin also supports index and multi index creation.
They will be used to speed up queries:

![Index Creation](<../.gitbook/assets/image (183).png>)

## Compression

### Defining a segmentation column (Optional)

To compress a hypertable, we first must configure its segmentation column.
This is not mandatory, as a hypertable can be compressed without having a segmentation column defined.
This will cause the compressed data not to be grouped.

One important thing to be aware is that **the segmentation column can be configured only before the data is compressed**. Never after.
In order to define a segmentation column for a compressed hypertable, it'll have to be decompressed first.

![Defining segmentation column](<../.gitbook/assets/image (184).png>)

It's also possible to define a set of segmentation columns for the same hypertable, by separating the with commas like `(device_id, time)`.

### Compressing a hypertable

After the segmentation column has been defined, the hypertable can be compressed by clicking on the `Compress` button:

![Compressing the hypertable](<../.gitbook/assets/image (185).png>)

### Defining a compression policy

With TimescaleDB plugin, it's also possible to define a compression policy.
This works like a recurrent background process that compresses a specific hypertable after its chunks reaches a given age.

This is useful to avoid recurrent manual compressions on the hypertable, and also to
decrease the space consumed by older data, which are not queried often.

![Compression policy](<../.gitbook/assets/image (186).png>)

## Listing hypertable chunks

After a hypertable is created, it's automatically partitioned into chunks (periods of time).
The chunk interval is actually pre-defined to contain **7 days of data**.

Supposing we have one month of historical data in a hypertable, the first chunk will contain data from days 1ª to 7º,
the second chunk from days 8º to 14º, and so on. More chunks are automatically created when needed.

It's possible to view the created chunks by using the `View Chunks` button, making it also possible
to monitor which chunks are currently compressed or not:

![Listing chunks](<../.gitbook/assets/image (187).png>)