# Multi-value snapshot

Those widgets are capable of plotting different values of the current output of a Pipes query.

{% hint style="info" %}
They are called snapshot widgets since they plot only the data on one output batch, rather than all the timespan.
{% endhint %}

A query like the one below would populate a chart.

```
=> [
"Sector 1" as type, random() as v, '#4CAF50' as __color every 1 minute
union
"Sector 2" as type, random() as v, '#3498db' as __color every 1 minute
union
"Sector 3" as type, random() as v, '#e67e22' as __color every 1 minute
union
"Sector 4" as type, random() as v, '#607D8B' as __color every 1 minute
union
"Sector 5" as type, random() as v, '#c0392b' as __color every 1 minute
]
=> v, __color by type every 1 minute
```

## Bar

![Example of a bar chart](<../../.gitbook/assets/image (163).png>)

## Column

![Example of a column chart](<../../.gitbook/assets/image (27).png>)

## Pie

![Example of a pie chart](<../../.gitbook/assets/image (16).png>)

## Funnel

![Example of a funnel chart](<../../.gitbook/assets/image (65).png>)
