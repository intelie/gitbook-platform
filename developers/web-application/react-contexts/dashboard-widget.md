---
description: >-
  Dashboard widget specific context keeps the widget container title on this
  dashboard as well as its consolidated state after view mode modifications
---

# Dashboard widget

### Context value

```typescript
type DashboarWidgetContext = {
    title: string
    setTitle: string => void
    widget: LiveWidgetEntity // the consolidated widget configuration
    state: LiveDashboardWidgetState // LiveDashboardWidgetState
}
```

### Usage

```jsx
import React, { useContext } from 'react'
import { DashboardWidgetContext } from 'live/context/dashboard-widget'

export default () => {
    const context = useContext(DashboardWidgetContext)

    function updateTitle() {
        context.setTitle(<span style={{color: 'red'}}>My title</span>)
    }
    return <a onClick={updateTitle}>change title</a>
}
```

### Other Widget Types

```typescript
declare type LiveDashboardWidgetState = {
    params: {
        spanFinal: string,
        spanDetached: boolean,
        span: string
    }
    state: {
        loading: boolean,
        receivedData: boolean,
        started: boolean,
        error?: {
            remote: boolean
        },
        lastUpdate?: number,
        lastEventTimestamp?: number,
        layout: {
            title: boolean,
            timespan: boolean,
            lastUpdate: boolean
        }
    }
    widgetTransientParams?: LiveWidgetConfig
    widget: LiveWidget
}
```
