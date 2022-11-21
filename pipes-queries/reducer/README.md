# Reducer

A Pipes query is performed sequentially on the historical data and on the real-time data.

A reducer is an extra step that can be used on any Pipes query to reduce the size of the historical results for optimization purposes. A reducer can be any Pipes aggregation, such as the last result, an average or a sum, or a compression version of the historical data.

Let's say that one wants to plot only the last value of an aggregation, which can be updated every second. For the real-time, it makes sense to update the value every second. On the other hand, we need to show only the last output value for the historical series. This can be achieved using the [`@latest`](https://pipes.intelie.com/docs/#pipe-latest) pipe.

```
@latest
```

As another example, let's say that one wants to run a query that plot a chart of a variable with a sampling rate of 1 sample per second over a period of one year. This would lead to 31.536.000 points for a single series. As a regular monitor does not have enough pixels to plot this amount of points, they should use the[ `@compress`](https://pipes.intelie.com/docs/#pipe-compress) pipe to reduce that amount to a value close to the number of available pixels.

{% hint style="info" %}
On internal tests, we found that generally a number of 640 points is enough for a regular series on any monitor.
{% endhint %}

In that case, the expression of the reducer could be something like:

```
@compress 640, _
```

If the expression has any [grouping](https://pipes.intelie.com/docs/#concepts-grouping), the more generic expression shown below would serve to any common case.&#x20;

```
@compress 640, 6400, (itermap(*) |> value#) by *[@@0]:object:compile.map(explode @@group)
```

## Data compression

Intelie Live provides two default functions to compress the data: [uniform compress](uniform-compress.md) (default) and [PIP](pip.md).

The uniform compression is the default algorithm for the `@compress` pipe in Intelie Live. The algorithm behind the pipe can be changed in the system web administration, as the figure below shows.

![The compression algorithm can be changed on the web interface](<../../.gitbook/assets/image (144).png>)

The objective of a point compression algorithm is to reduce the number of points from a series, while keeping the most relevant points. The traditional compress algorithm is Perceptually Important Points (PIP), which keeps points with the greatest distance to a line between two other points. The PIP algorithm favors "noisy" parts of a series, keeping more points there in detriment to "quiet" parts. When the number of points exceeds the screen resolution, a "noisy" part can have more points than can be displayed, while a "quiet" part can have too few points to keep a good approximation of its shape.
