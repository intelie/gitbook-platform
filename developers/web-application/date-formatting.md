---
description: >-
  It is possible to choose the date formats used on the whole system using
  Live's Date service.
---

# Date formatting

To customize date formats system-wide you can do it via plugin or create a **Admin > Interface Customizations > JS Snippet** to apply a new set of [date formats.](https://momentjs.com/docs/#/displaying/format/)

```typescript
var setDefaultDateFormats = require('live/services/date').setDefaultDateFormats

// these are the default values
setDefaultDateFormats({
    date: {
        pt_br: 'DD/MM/YYYY',
        en_us: 'MM/DD/YYYY'
    },
    dateTime: {
        pt_br: 'DD/MM/YYYY HH:mm:ss',
        en_us: 'MM/DD/YYYY hh:mm:ss A'
    },
    dateShortYear: {
        pt_br: 'DD/MM/YY',
        en_us: 'MM/DD/YY'
    },
    time: {
        pt_br: 'HH:mm:ss',
        en_us: 'hh:mm:ss A'
    },
    timeShort: {
        pt_br: 'HH:mm',
        en_us: 'hh:mm A'
    },
    timeMask: {
        pt_br: '11:11',
        en_us: '11:11 AA'
    }
})
```

You have total control over the **JS Snippet** so if you you need any logic for applying different sets, this is the place.
