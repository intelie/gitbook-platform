# Temporal

The temporal chart supports several different series styles, such as line, spline, bars, stacked bars, area, range, bubbles, scatter, flags and polygons. Each series can have different style and color on the same chart.

![A few temporal charts on a dashboard](<../../.gitbook/assets/image (62).png>)

Any numeric value output from a Pipes query will populate the chart. Text values are also accepted, in case of flags. A query that genertes random data, such as the one below, can be a good starting point.

```
=> random(30000,90000):round(2) as value every minute
```

All the layout configuration is performed via web interface.



![Configuring the chart type and color](<../../.gitbook/assets/image (31).png>)

The charts are highly flexible on the layout configurations.

![An example with logarithmic axes, vertical layout and grouped axes labels](<../../.gitbook/assets/image (141).png>)
