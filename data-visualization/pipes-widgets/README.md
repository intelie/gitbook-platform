# Pipes widgets

INTELIE Live provides a set of standard widgets out-of-the-box. Those widgets require Intelie Pipes for the data handling.

All of the widgets provide support for many layout configuration via web interface.

## Data Layers

Some widgets support more than one layer, that is, more than one pipes query running simultaneously. In that case, the output of the different queries are plotted together on the chart.

## Reducer

The reducer is an optimization. It is needed to limit the amount of data delivered to the chart, since it is often impossible to plot all the desired data. For more details, see [this section](../../pipes-queries/reducer/).

In case that the widget supports more than one layer, each one must have its own reducer.
