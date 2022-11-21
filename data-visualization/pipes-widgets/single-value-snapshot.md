# Single-value snapshot

Those widgets are capable of plotting one value of the last output.

{% hint style="info" %}
They are called snapshot widgets since they plot only the data on one output batch, rather than all the timespan.
{% endhint %}

The query below would populate those widgets.

```
=> random() as value every minute
```

## Value

![Example of a value widget](<../../.gitbook/assets/image (122).png>)

## Gauge

![Example of a gauge widget](<../../.gitbook/assets/image (113).png>)
