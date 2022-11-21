# Uniform compress

This algorithm attempts to keep the number of points on the series somewhat uniformly distributed on the independent variable axis. This algorithm groups the points in non-overlapping intervals, keeping only the most relevant points from each interval.

To compress the series, pairs of adjacent intervals are merged until the desired total number of points is reached.

There are two sub-algorithms. The first decides which intervals to merge, based only on the independent variable. The second decides which points to keep in the merged interval.

As this algorithm initially splits the series into equivalent sized buckets, it does not tend to choose more points on any specific area, leading to better visual results.
