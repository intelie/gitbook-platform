---
description: TimescaleDB expands PostgreSQL for time series and analytics.
---

# TimescaleDB

## TimescaleDB Plugin

TimescaleDB plugin is built over PostgreSQL and handles relentless streams of time-series data with the performance, scalability, and usability that the application needs.

This document provides an overview of the new timescaledb plugin and the features of the recently released 2.0.0 version.
In addition, it describes the features planned for the upcoming version.

TimescaleDB plugin was designed with two main goals: Improve query performance and reduce storage costs, when compared to other INTELIE Storage Providers.
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
Those partitions are called "chunks", which is the **smallest portion of data that can be compressed and decompressed**.
The chunks can be considered many child tables, which compose the hypertable.

![Hypertable chunks](<../.gitbook/assets/image (178).png>)

### Compression

Compression reduces the amount of space taken up by your data. For some queries, it also speeds up query time, because fewer bytes need to be read from disk.
TimescaleDB uses native compression for hypertable data. That means it doesn't need a specific file system or extra software. Compression works out-of-the-box.
**Compressed data is stored in a hybrid row-columnar format**. This format works best for older data, which is queried but rarely changed.
Newer, often-changed data should be stored in uncompressed row format.

- Uncompressed Chunk:

![Uncompressed hypertable](<../.gitbook/assets/image (179).png>)

- Compressed Chunk:

![Compressed hypertable](<../.gitbook/assets/image (180).png>)

### Segmentation

Data compression allows to choose a column on which the data will be segmented by.
Segment columns define how **the data is grouped when compressed** in TimescaleDB.

In other words, each row in a compressed table must contain data about a single item.
The column that a table is segmented by contains only a single entry, while all other columns can have multiple arrayed entries.
For example, if a hypertable is segmented by device ID, this is how the data will be arranged after compressed:

![Segmented hypertable](<../.gitbook/assets/image (181).png>)

Unlike above, each compressed row has information about a single device ID.
This can be particularly efficient on queries that use the segment column in the WHERE clause, because the decompression can happen after filtering.

It's important to note that **segment columns are not compressed**.
Instead, for each value of the segment column, a separate compressed row is created.

## Hypertable's Management

The event types can be managed in the **Hypertables** tab.
Each INTELIE Live event type is mapped to a hypertable. 
Besides the basic operations to view index details and drop the hypertable, more advanced features are provided for compression, segmentation and chunk listing.

![Hypertable Management](<../.gitbook/assets/image (182).png>)

## Index Creation

TimescaleDB plugin also supports index and multi index creation.
They will be used to speed up queries.

![Index Creation](<../.gitbook/assets/image (183).png>)

## Compression

### Defining segment columns (Optional)

Segmenting is the very first decision to take before compressing a hypertable.
Once the compression is configured, the segmenting can only be edited by decompressing the entire hypertable again.

The segments are optional since a hypertable can be compressed without it.
In this case, all columns will be grouped.

Note **the segment columns can be configured only before the data is compressed**. Never after.

![Defining segment columns](<../.gitbook/assets/image (184).png>)

It's also possible to define a set of segment columns for the same hypertable, by separating them with commas like `(device_id, time)`.

The segmenting jobs started in this screen can be managed in the Jobs tab bellow.

### Compressing a hypertable

After the segment columns have been defined, the hypertable can be compressed by clicking on the `Compress` button:

![Compressing the hypertable](<../.gitbook/assets/image (185).png>)

The compression jobs started in this screen can be managed in the Jobs tab bellow.

### Defining a compression policy

With TimescaleDB plugin, it's also possible to define a compression policy.
This works like a recurrent background process that compresses a specific hypertable after its chunks reaches a given age.

This is useful to avoid recurrent manual compressions on the hypertable, and also to
decrease the space consumed by older data, which are not queried often.

![Compression policy](<../.gitbook/assets/image (186).png>)

The policy compression jobs started in this screen can be managed in the Jobs tab bellow.

## Jobs Management

Starting with plugin version 2.0.0, each compression task (i.e, compression, decompression, segmenting and compression policy)
runs asynchronously. This avoids issues when trying to start concurrent jobs manually and allows
the administrator to keep track of each scheduled job.

After scheduling any of the above compression tasks, a new row will be added to the Jobs tab:

![Jobs Management](<../.gitbook/assets/image (188).png>)

This tab delivers job related information, like the job type, associated hypertable, the job status, start and end date.
It's also possible to filter the list of jobs based on any of those fields:

![Jobs Filtering](<../.gitbook/assets/image (189).png>)

After a job is started, it can assume one of the following status:

- Scheduled - The job is registered in the execution queue, but hasn't started yet
- Running - The job is currently running
- Succeeded - Job execution completed successfully
- Failed - Job execution completed with failures
- Recurrent - This status is reserved for policies, which keep running indefinitely in the background
- History - After a hypertable is removed, its related jobs assume the History status for query purposes

**PS:** Currently, compression policy jobs will always have Recurrent status and never get updated.

## Listing hypertable chunks

After a hypertable is created, it's automatically partitioned into chunks (periods of time).
The chunk interval is actually pre-defined to contain **7 days of data**.

For example, one month of historical data in a hypertable means the first chunk will contain data from days 1 to 7,
the second chunk from days 8 to 14, and so on. More chunks are automatically created when needed.

It's possible to view the chunks created by using the `View Chunks` button, making it also possible
to identify which chunks are currently compressed or not:

![Listing chunks](<../.gitbook/assets/image (187).png>)

## What we have achieved and what we can expect

### TimescaleDB vs Vanilla PostgreSQL

In our experiments we have successfully **reduced the storage size by up to 93%** when compared to the equivalent dataset on PostgreSQL plugin.

While in plain PostgreSQL the data occupied 36.67 GB, using TimescaleDB the space consumed decreased to 2.88 GB (with the hypertable fully compressed).

### TimescaleDB vs MongoDB

Our experiments also demonstrated that the storage size was **reduced by up to 59%** when compared to the same dataset on MongoDB.

While in plain MongoDB the data occupied 226 GB, using TimescaleDB the spaced consumed decreased to 92.49 GB (with the hypertable fully compressed).

In addition, for some queries, we were able to obtain a **performance gain of up to 38%**.

### Remarks

While the storage cost will much likely reduce by using TimescaleDB plugin, the query performance will depend on a few other factors.


The size of the chunk interval, segment columns, created indexes, data ordering,
data characteristics, aggregations, as well as data distribution are parameters to be considered
that should interfere with the good or poor performance of a query.

In our experiments, TimescaleDB has shown to extract a lot of performance in queries with heavy aggregations over time series.

### Future plans and releases

#### Support to decompression using the UI (1.6.0 / 2.1.0)

At the moment (version 1.5.0 / 2.0.0), it's not possible to decompress a hypertable using the UI.
We are working on it to add a decompression button to the hypertable management tab.

#### Job Cancellation

Currently, it's not possible to cancel a job execution after it has started.

We plan to develop a new feature to deliver this capability soon.

## Requirements

Currently, the TimescaleDB plugin requires a TimescaleDB 2.5+ extension to work properly.

## References

Recommended reads from official TimescaleDB documentation:

- [Time series Data](https://www.timescale.com/blog/time-series-data/)
- [Installation how-to](https://docs.timescale.com/install/latest/)
- [Getting started](https://docs.timescale.com/getting-started/latest/)
- [Overview](https://docs.timescale.com/timescaledb/latest/)
- [API reference](https://docs.timescale.com/api/latest/) (for administrators)