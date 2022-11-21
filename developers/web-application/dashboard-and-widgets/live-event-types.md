# Live Event Types

The data event type is a single type itself:

**event** : describes the batch of `events` itself, so the output depends on pipes expression. When the `type` parameter is `event` so the `events` parameter is an array of events (a batch). Besides its own format, every event arrives with a `timestamp` field indicating when the event occurs.

The control event types are described below:

**start** : describes the fields produced by the pipes query and its details (time window clause, estimate weight in bytes, etc)

```javascript
{
  "fields": {
    "timestamp": "java.lang.Double",
    "random": "java.lang.Double"
  },
  "group": [],
  "select": [
    "random"
  ],
  "type": "start",
  "info": {
    "type": "row(number timestamp; ; number random)",
    "output": [
      {
        "amount": 10,
        "unit": "SECOND",
        "type": "time",
        "zone": "America/Sao_Paulo"
      }
    ],
    "safe": true,
    "weight": 64,
    "window": "every 10 seconds",
    "span": {
      "text": "1 hour before (now)",
      "normalized": "1 hour before (now)",
      "fixed": false,
      "point": false,
      "includesPresent": true
    }
  }
}
```

**progress** : describes the progress of pipes query through the historical data collection (since some queries doesn't requires to search data in storage, this event control could result 0 as total)

```javascript
{
  "current": 0,
  "total": 0,
  "estimate": false,
  "rate": 0,
  "message": "Scheduling storage queries...",
  "type": "progress"
}
```

**warning** : describes some exceptional situation as the fact of the query used in this tutorial doesn't require any stored data

```javascript
{
  "severity": "INFO",
  "message": "The chosen filter requires no queries on the storage.",
  "type": "warning",
  "info": {
    "severityLevel": 1
  }
}
```

**span** : describes the begin and end of the time window to be observed

```javascript
{
  "begin": 1591057993379,
  "end": 1591061593379,
  "type": "span"
}
```

**performance** : describes which query was executed and some details as its duration (in miliseconds)

```javascript
{
  "count": 1,
  "time": 14,
  "storage": [],
  "live": [
    {
      "q": "pipes{=> random() every 10 sec}/reduce()/span(tz 'America/Sao_Paulo' (last hour))/nolistener/follow/preload/context{description=Editing Widget: tutorial test (21) / Dashboard: 3, url=/widget/21/#/edit, source={type=widget, kind=WIDGET, id=21, dashboardId=null, layerIndex=0, isBaseline=false}, user={id=1, username=admin, displayName=Amadeu Barbosa, email=live-dev@intelie.com}, host=127.0.0.1, timezone=America/Sao_Paulo, id=734, createdAt=1591061593371}",
      "url": "/widget/21/#/edit",
      "flowCount": 0,
      "flowTime": 0,
      "miscCount": 5,
      "miscTime": 13
    }
  ],
  "type": "performance"
}
```

**endHistory** : marks the end of the historical data analysis

```javascript
{
  "timestamp": 1591061593381,
  "duration": 15,
  "type": "endHistory"
}
```

**startRealTime** : marks the real-time events are coming next

```javascript
{
  "type": "startRealTime"
}
```

**stop** : marks the query was destroyed either by the lack of need to follow real-time (reason: "NoRealTime" in that case) or if it was explicitly destroyed (at administration interface for ex) or yet because the Pipes Expression failed to compile

```javascript
{
  "type": "stop",
  "message": "PipeException: At (1, 31->1, 33): Unexpected 'the'. Was expecting one of: <EOF>, 'at', 'every'... (6 more)",
  "reason": "Error",
  "user": false
}
```
