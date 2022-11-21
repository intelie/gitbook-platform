# Pipes modifiers on Pipes charts

Standard charts listen to Pipes modifiers.&#x20;

Some modifiers are available as [meta parameters](../pipes-queries/meta-parameters.md) and some are available as simple variables on the event itself (starting with `__`).

| Property                           | Type                                                     | Supported charts                                     | Default | Since           |
| ---------------------------------- | -------------------------------------------------------- | ---------------------------------------------------- | ------- | --------------- |
| `@meta.color` or `__color`         | string or map                                            | temporal, cartesian, bar, column, funnel, pie, value |         | @meta on 2.25.2 |
| `@meta.dashStyle` or `__dashStyle` | string                                                   | temporal, cartesian                                  |         | @meta on 2.25.2 |
| `@meta.lineWidth` or `__lineWidth` | integer                                                  | temporal, cartesian                                  |         | @meta on 2.25.2 |
| `__markerRadius`                   | integer                                                  | temporal, cartesian                                  |         |                 |
| `__markerEnabled`                  | boolean                                                  | temporal, cartesian                                  |         |                 |
| `__markerSymbol`                   | string                                                   | temporal, cartesian                                  |         |                 |
| `__dataLabelEnabled`               | boolean                                                  | temporal, cartesian                                  |         |                 |
| `__description`                    | string                                                   | temporal, cartesian                                  |         |                 |
| `__wstart`                         | long                                                     | temporal                                             |         |                 |
| `__xAxisMin`                       | long                                                     | cartesian                                            |         |                 |
| `__xAxisMax`                       | long                                                     | cartesian                                            |         |                 |
| `__clear`                          | boolean or map                                           | temporal, cartesian                                  |         |                 |
| `__syncTooltipField`               | string                                                   | temporal, cartesian                                  |         | 2.15.3          |
| `__formatted`                      | string or map                                            | temporal, cartesian, bar, column, funnel, pie        |         | 2.17.0          |
| `__columnWidth`                    | integer                                                  | ranking, feed                                        |         | 2.19.3          |
| `__uid`                            | string                                                   | feed                                                 |         | 2.20.6          |
| `__title`                          | string                                                   | temporal, cartesian                                  |         | 3.9.0           |
| `@meta.color`                      | map                                                      | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.enableDataGapsReport`       | boolean                                                  | temporal                                             |         | 2.28.0          |
| `@meta.dataGapsReportThreshold`    | long                                                     | temporal                                             | 60000   | 2.28.0          |
| `@meta.zIndex`                     | map                                                      | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.zones`                      | map\<seq\<obj>>                                          | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.legendIndex`                | map                                                      | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.hideInLegend`               | string or map                                            | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.disableStateMarker`         | boolean                                                  | temporal, cartesian                                  |         | 2.25.8          |
| `@meta.disableMouseTracking`       | boolean                                                  | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.lineColor`                  | string                                                   | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.zoneAxis`                   | `x` or `y`                                               | temporal, cartesian                                  |         | 2.25.2          |
| `@meta.polygon`                    | map                                                      | cartesian                                            |         | 2.25.2          |
| `@meta.yAxis`                      | seq\<map>                                                | temporal, cartesian                                  |         | 2.27.0          |
| `@meta.chartOptions`               | map ([ref](https://api.highcharts.com/highcharts/chart)) | temporal, cartesian, column, bar, gauge, funnel, pie |         |                 |
| `@meta.legendIndex`                | map(name, order)                                         | temporal, cartesian, column, bar, gauge, funnel, pie |         |                 |



{% hint style="info" %}
The `@meta` parameters must be used on the body of a query, for example `=> @meta value as property`.
{% endhint %}

{% hint style="info" %}
`@meta` modifiers can be set globally or individually.

`=> @meta "red" as color`will make all series red.`=> @meta newmap("my-series-name", "red") as color` will make "my-series-name" red.
{% endhint %}

## Examples

#### @meta.yAxis

Defines the y axes configurations for the chart.

```
=> 
  @meta (
    newmap(
      "label", "a",
      "color", "red", 
      "min", 20,
      "max", "auto",
      "position", "right",
      "uom", "m",
      "fields", ("a"):seq
    ),
    newmap(
      "label", "b",
      "color", "blue", 
      -- "min", 100,
      "max", 200,
      "position", "right",
      "uom", "m",
      "fields", ("b"):seq
    ),
  ):seq as yAxis
```

#### event.\_\_formatted

A formatted string that will be used to display the field value.

```
@set newmap("a", "$ 56.87") as __formatted
```

#### @meta.zIndex

Changes series zIndex, use in case you want to a make a field overlap another.

```
=> @meta newmap("a", 1, "b", 2) as zIndex
```

#### @meta.hideInLegend

Changes hide selected series on the chart legend.

```
=> @meta newmap("a", true) as hideInLegend
=> @meta newmap("a", true, "b", true) as hideInLegend
=> @meta true as hideInLegend -- hide all legends
```

**@meta.zones**

Create value thresholds that defines a new color for the field above that value.

```
=> @meta newmap(
    "a", 
    (newmap(
        "color", "green",
        "value", 0.5
    )
    ):seq) as zones
    
-- for the field "a" above value 0.5, field should be color "green"
```

**@meta.color**

Changes a field color

```
=> @meta newmap("a", "red", "b", "blue") as color
```

**@meta.lineWidth**

Defines the line width for the selected series.

```
=> @meta newmap("a", 1, "b", 2) as lineWidth
=> @meta 4 as lineWidth -- apply to all series
```

**@meta.legendIndex**

Defines the legend and tooltip order on charts

```
=> @meta newmap("series-a", 1, "series-b", 2) as legendIndex
```

**@meta.dashStyle**

Changes the default dash (Solid) style for series.

```
=> @meta newmap("a", 'Dash') as dashStyle
=> @meta 'Dash' as dashStyle -- apply to all series
```

{% hint style="info" %}
Accepted **dashStyle** values**:**

* Solid
* Dash (DashDot)
* Dot
* LongDash (LongDashDot, LongDashDotDot)
* ShortDash (ShortDashDot, ShortDashDotDot)
* ShortDot
{% endhint %}

**@meta.polygon**

```
=> @meta 
    newmap(
        "color", "rgba(255, 0, 0, 0.3)", 
        "points", "[[1, 2], [2, 3], [4, 3]]"$:jsonparse()
    ) as polygon
```

**@meta.enableDataGapsReport and @meta.dataGapsReportThreshold**

This query generates the chart in the image below.

```
=> sin(otimestamp() / pi() / 36000) as r, (ostart():dateformat('mm')#) as min every second
=> @filter min % 3 != 0
=> r
=> @meta true as enableDataGapsReport, 61000 as dataGapsReportThreshold
```

![Data gaps demonstration](<../.gitbook/assets/image (2).png>)
