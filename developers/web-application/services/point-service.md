---
description: >-
  Used on Dashboards to synchronize the visualization of the current hovered
  point.
---

# Point service

{% hint style="info" %}
Since Live @ 3.0.0
{% endhint %}

```typescript
type CurrentPoint = {
    x: number | null
    source?: string
    chart?: Highcharts.Chart | Record<string, any>
    event?: MouseEvent | any
}

type ChartPoint = Highcharts.Point | LiveChartPoint
type HoverPointsKey = Highcharts.Chart | Record<string, unknown>
type HoverPointMessage = { key: HoverPointsKey; points: HoverPoints }
type HoverPointsState = WeakMap<HoverPointsKey, HoverPoints>
type HoverPoints = Array<ChartPoint>

export class PointService {
    currentPointBehavior$: BehaviorSubject<CurrentPoint>
    currentPoint$: Subject<CurrentPoint>
    #hoveredPointsMap = WeakMap<Highcharts.Chart | Record<string, unknown>, HoverPoints>
    hoverPointsBehavior$ = BehaviorSubject<HoverPointsState>
    hoverPoints$ = Subject<HoverPointMessage>
    
    getClosestPoint: (
        points: Array<ChartPoint> | void,
        point: LiveChartPoint | { x: number },
        syncTooltipField?: string,
        allowNullY = true
    ) => ChartPoint
}
```
