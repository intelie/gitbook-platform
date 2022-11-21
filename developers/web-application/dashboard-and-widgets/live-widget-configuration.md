# Live Widget Configuration

Live Widgets have a sort of configuration filled by Live Dashboard when the Live Widget instance is created either by the editor or by the dashboard viewer.&#x20;

```typescript
declare type LiveWidgetConfig = {
    properties?: Array<LayerConfig>
    span?: string
    ticks?: string | Array<number>
    colorGradient?: ColorGradient
    disableCompress?: boolean
    disableTruncate?: boolean
    isDatetime?: boolean // defines if the xAxis should be formatted as date
    isRanking?: boolean
    hideGridLines?: boolean
    hideTooltip?: boolean
    hideXAxis?: boolean
    hideYAxisLabels?: boolean
    hideYAxisValues?: boolean
    legendEnabled?: boolean
    legendRight?: boolean
    logarithmicScale?: boolean
    minValue?: number
    maxValue?: number
    showHeader?: boolean
    showTimestamp?: boolean
    verticalEnabled?: boolean
    xAxisAlign?: string
    xAxisOpposite?: boolean
    xAxisTitle?: boolean
    xCellSize?: number
    yAxisLabelsMode?: 'grouped' | 'independent'
    yAxis?: Array<LiveYAxisConfig>
    yCellSize?: number
    zoomEnabled?: boolean
}

declare type LiveWidgetImplOptions = {
    activeFilters: Record<string, any>
    id: number | string
    name: string
    cfg: LiveWidgetConfig
    mode: 'editor' | LiveDashboardModes
    onReady: typeof Promise.resolve
    onSelection?: Function
    onChangeUI?: Function
    widgetIndex: number
    dashboardTheme?: LiveDashboardTheme
    reloadWidget: (id: string) => void
    context: LiveWidgetConfigContextValue
    dashboardContext: DashboardContext
    dashboardWidgetContext: any
}

declare type LayerConfig = {
    chartType: string
    reducerType: 'auto' | 'manual' | 'off'
    query: {
        expression: string
        reducer: string
    }
}
```

The widget instance is injected by the both `cfg`and `options`properties typed respectively as `LiveWidgetConfig` and `LiveWidgetImplOptions` as show in code snippet above. So the developer just handle the `this` object to use these properties.

As mentioned in [WidgetService](live-widget-api.md#widgetsservice) section, the widget implementation also could provide some specific configuration, that object will be used to inject all other default widget configuration properties. So, it's important to avoid property override to ensure the expected configuration behavior.
