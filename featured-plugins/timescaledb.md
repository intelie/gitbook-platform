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
