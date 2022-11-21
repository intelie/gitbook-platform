---
description: >-
  Live provides an API where plugins can intercept and alter any widget request
  before it reaches the backend.
---

# Widget Request Interceptors

### The Widget Request Interceptor API

This API let's you register a function that will be called before a widget request is made, this function has complete control over the widget request object and may return a promise. Widgets are constantly observing the list of interceptors and will reload everytime a intercepor is added or removed.

### How to use

Imagine that the _plugin-number-precision_ wishes to change every widget request to include the\
`{ precisionLevel: HIGH }` parameter which is passed down to the query and causes the pipe engine to output every numeric value with the highest precision possible.

This can be achieved by using a widget request interceptor as follows:

```typescript
// plugin-number-precision/src/webapp/main.ts

import { LiveApi } from 'live/lib/api'

// This function will be called before every widget request is made.
function includePrecision(widgetRequest, dashboardParams) {
    // Don't change anything on edit mode 
    if (dashboardParams?.mode === 'edit' ) {
        return widgetRequest
    }
    
    // Add custom parameter
    widgetRequest.userConfig.precisionLevel = 'HIGH'
    
    return widgetRequest
}

LiveApi.Interceptor.widget.requests.register(includePrecision)
```
