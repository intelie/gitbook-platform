---
description: >-
  The Dashboard context keeps the dashboard store state as well as its actions
  that changes the dashboard state.
---

# Dashboard

### Context value

```typescript
type DashboardContext = {
    dashboard: LiveDashboard
    theme: LiveDashboardTheme
    params: {
        span: string,
        mode: LiveDashboardModes
    }
    highlightLock: boolean
    setHighlightLock: (lock: boolean) => void
    highlightedWidgetId?: number
    setHighlightedWidgetId: (id: number) => void
    actions: DashboardActions
    state: LiveDashboardState
}
```

### Dashboard Actions

```typescript
declare type DashboardActions = {
    fetchDashboard(id: string, restartSubscriber: boolean): void,
    setDashboard(data: Dashboard): void,
    saveDashboard(object: Dashboard): void,
    saveWidget(widget: LiveWidget, requestIndex: ?number, cb?: Function): void,
    changeMode(mode: $PropertyType<DashboardQueryParams, 'mode'>): void,
    duplicateWidget(widget: LiveWidget, cb?: Function): void,
    removeWidget(id: string): void,
    start(requestIndex: number): void,
    stop(requestIndex: string): void,
    setFilterConfig(config: FilterConfig): void,
    setInactiveFilterConfig(): void,
    setFilterValues(values: { [string]: string }): void,
    setTimespan(span: string, requestIndex: number, restart: boolean): void,
    updateSpan(action: 'apply' | void, span: string, requestIndex: number, percentage: number): void,
    setOrChangeTheme(theme: LiveDashboardTheme): void,
    attachOrDetach(requestIndex: number): void,
    detachedInViewMode(widgetId: string): void,
    resetDetachedInViewMode(widgetId: string): void,
    updateWidgetLayoutOptions(requestIndex: number, property: string, value: mixed): void,
    resizeLayout(): void,
    forceResizeLayout(): void,
    resetDefaults(): void,
    resizeWidgets(): void,
    disableTip(type: string): void,
    toggleWidgetError(requestIndex: number, event: Object): void,
    updateWidgetViewConfig(id: string, config: LiveWidgetConfig, sideEffects: { updateQuery: boolean }): void,
    restoreWidgetViewConfig(id: string): void,
    toggleWidgetConfigApprove(id: string, approved: boolean): void,
    updateWidgetUI(id: string, type: string, config: Object): void,
    updateUserLayout(layout: LayoutConfig): void,
    updateLayoutConfig(config: LayoutConfig): void,
    setDeviceType(device: Device): void,
    applyLayoutTo(allOrCurrent: 'all' | 'current'): void,
    setHiddenWidgets(hiddenWidgets: number[]): void,
    updateHiddenWidgets(id: string): void,
    hideLayoutAlert(): void,
    changeSpanPickerConfig(id: string, type: string, config: Object): void
}

```

### Usage

```jsx
import React, { useContext } from 'react'
import { DashboardContext } from 'live/context/dashboard'

export default () => {
    const context = useContext(DashboardContext)

    function updateSpan() {
        context.actions.updateSpan('apply', {
            start: 1,
            end: 100
        }, 0)
    }
    return <a onClick={updateSpan}>change span</a>
}
```

{% hint style="warning" %}
Warning: The dashboard context won't be available in the widget editor. It'll only be available in the dashboard
{% endhint %}

### Other Dashboard Types

```typescript
declare type LiveDashboardTheme = 'light' | 'dark'
declare type LiveDashboardModes = 'view' | 'edit' | 'fullscreen'
declare type LiveDashboardState = {
    object: LiveDashboard,
    warning?: string,
    canEdit: boolean,
    spanLabel: string,
    running: boolean,
    storage: {
        items: { [id: string]: any },
        keys: { [id: string]: any }
    },
    params: {
        span: string,
        mode: LiveDashboardModes
    },
    filterValues: LiveFilterValues,
    currentFilterValues: LiveFilterValues,
    progress?: number,
    progressTotal?: number,
    progressETA?: number,
    progressObj: {
        current: number,
        total: number,
        estimate: boolean,
        rate: number,
        message: string
    },
    loading: boolean,
    globalLoading: boolean,
    selectSpan: boolean,
    lastUpdate: number,
    showLayoutAlert: boolean,
    showFilterAlert: boolean,
    layoutPreferencesChanged: boolean,
    savingDashboard: boolean,
    currentSpan: string, // same as params.span
    currentDevice: LiveDeviceType,
    deviceEditType: LiveDeviceType,
    applyLayoutTo: 'all' | 'current'
}
```
