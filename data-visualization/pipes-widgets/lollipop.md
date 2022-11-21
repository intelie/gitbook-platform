# Lollipop

A Temporal Lollipop is a way of representing events that consist of some numerical values that occurs over time.

Visually, the lollipop chart consists of a line (or a thin bar) with a circle on top of it. The position of the bar will represent the timestamp of the event and the height of the bar will represent its value.

So, Temporal Lollipop charts are two-dimensional with two axes: one axis for the timestamp of the event, and the other axis for its numerical value. Those numerical values are indicated by the position of the circle on top of the line.

It is particularly useful to show events that occur close in time, but whose duration can generate overlapping drawings if, for example, a timeline were used. Thus allowing us to visualize events very close together, without losing the information about their duration.

{% hint style="info" %}
This functionality is provided by the **live-exploratory-viz** plugin. It is available on the [marketplace](https://marketplace.intelie.com/artifact/plugin-live-exploratory-viz/).
{% endhint %}

**Functional description**

*   The result of the query pipes must have at least one numerical value related to a timestamp:

    `=> random() as value every 5 sec`

![The widget works in vertical orientation and also in horizontal orientation.](https://lh4.googleusercontent.com/1ORHhYPODdIhiEESHIpwIpQuukkNwGYVdel53M526JneNi0arxDdh3BA-F73jsDgvr3b91I54tCO0y6NJufNc1mXN09v5J4K3bBb1u5glBXDyHbk9nhlB02AUqQYxYBf8luly5vVLyZZamQiVg)

* As a temporal version of the lollipop chart, the x-axis always refers to a time period (timestamp):

![The widget works in real time and also in closed time periods](https://lh4.googleusercontent.com/H8aKf2Aia-V2Oi9tc\_PkSU-Z1sJB8e7SbcNxBqtkCG-6kSoiOFXjPJrQBXt45\_OGX3iomHOpHYPP1CNh-jG26F7pIrxvJpDrwm6pHhiGCyOtgeRY4qzuxMHGdQK0oeQQ1cXS-sjheWNWcirNVQ)

*   We can modify the group colors by inserting the `__color` variable in the query pipes, like:

    `=> random() as value every 5 sec` \
    `=> @set value > 0.5 as category` \
    `=> @set value < 0.5 ? '#C0392B', '#607D8B' as __color` \
    `=> value, __color by category`

![](https://lh4.googleusercontent.com/ccmeQLeeYK1e0olCWNB3BiCL-3h5YrBIywiA7n\_eMADyHJjSVO88bruN6OkW31zgBNcyNh--e19A8YCGvEp6vyFrpDberJHdTiNaZEz01QKMIYq0SGeBsOhIeIcNhR8SuQj1DTFaeh8mOT2WGQ)

*   We can use other symbols besides circles, which can display category information more intuitively. So, we can modify the group markers in the query pipes, inserting the variables `__markerSymbol` to change the symbol type and `__markerRadius` to change the symbol size, like:

    `=> random() as value every 5 sec` \
    `=> @set value > 0.5 as category` \
    `=> @set value < 0.5?'#C0392B', '#607D8B' as __color` \
    `=> @set value < 0.5? 'square', 'diamond' as __markerSymbol` \
    `=> @set 6 as __markerRadius` \
    `=> value, __color, __markerSymbol, __markerRadius by category`

![](<../../.gitbook/assets/image (85).png>)

* Possible values of `__markerSymbol` are `'circle'`, `'square'`,`'diamond'`, `'triangle'` and `'triangle-down'.`
*   For correct group creation, these variables must always be grouped in the query pipes by the by clause, like:

    `=> value, __markerSymbol,__markerRadius, __color by category`
* The categories serve as captions and can be located on bottom, right or hidden.

![Bottom Legend](https://lh3.googleusercontent.com/wxnCPSN7KnDKzC4OkkzzUhTSLdl-zDhQ9LfOfCavVi1jof0usYG4M2cwTyqJdgN1jKgU56x-d5rqDo8gzeRNV8SphGS0R3l9KPrYi6cOMGkdS6Ko31iDBjqb6q6UlZwYqxOhwODu8aLO1B7OQg)

![Right Legend](https://lh3.googleusercontent.com/RCk0ZPSz-o3Y1Sbcs26KsKPZjfJSXO9mjKl9Y29Zd--v0tnS6A2jXsIJgB\_xq7r5Sd0ENI7k48wcgU2-Aw5DwPyY6Rjm5DTsg4eKB9kK7uf7L94LeNkVvzDSTqJSstm9Zpc2shDJ873cKxSCuQ)

![Hidden Legend](https://lh3.googleusercontent.com/BJI3QWvG5-p5B2OFTBGVk4eLHKyt80GHKnCDc-bOlCZoWB50VDcM40\_F4d3Hbce3-766X55b2hJyswZuDoE9apdoIZvHataMRs1z30YaY5xHn3D7Qdwy3CBwS0QvT5kSO2DF0djY4vKSI-5zHQ)
