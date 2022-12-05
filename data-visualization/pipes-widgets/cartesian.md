# Cartesian

The cartesian chart supports plotting 2D data. It is more generic than the temporal widget since it allows plotting other variables than the timestamp on the X axis. It also supports plotting the timestamp.

In cartesian charts, it is mandatory to define which is the variable to be plotted on the X axis. All other variables will be plotted on the Y axis.

![Defining the variable on the X axis](<../../.gitbook/assets/image (33).png>)

The Pipes query below would populate the chart on this example.

```
=> random() as value, random() as my_x_property every minute
```
