---
description: Live provides an API to register new charts implementations.
---

# Widget API

## Standard API

{% hint style="info" %}
Depends on [Live 2.27.0](https://platform.intelie.com/release-notes#2-27-0-jul-2-2020)
{% endhint %}

## Widgets service

The `widgetsService` is available in `live/services/widgets`. It is javascript module that allows registering custom widget implementation to Live run-time. Every widget must have a **unique string to represent its type**. In this tutorial example we choose **`my-simple-chart`** as shown below:

```typescript
// Widgets service usage:
import i18n from 'live/services/i18n'
import widgetsService from 'live/services/widgets'

// The widget implementation
import SimpleChart from './my-simple-chart'
const type = 'my-simple-chart'

// Examples on: "Registering the widget" sections
widgetService.register(...)
```

## Creating the Widget types

First we need to create types. If you do not have all definitions in mind at first, don't worry, create some placeholder types and enrich them at a later point.

{% code title="types.ts" %}
```typescript
// 1. Typing the widget State shape:
// State will be the consolidated value derived
// from the events flow by the event reducer
// on this example we'll keep this simple shape:
type ChartState = {
    value: number
}

// 2. Typing data events
// The shape of these events in runtime
// are defined by the query of the widget

// a. Event shape
type ChartEvent = LiveEvent<{
    fieldA: number
    fieldB: number
}>

// b. Describe your message shape
type ChartMessage = FlowMessage<ChartEvent>
 
 
// 3. Typing the widget configuration:
type Configs = {
    widget: LiveWidget<{ color: string }>
}
```
{% endcode %}

## Creating the Data Reducer

To get from events a consolidated current state we offer the abstraction of a **data reducer**. It is as simple as a switch-case statement that will always return an updated state value from each message received.

{% code title="reducer.ts" %}
```javascript
const SimpleReducer: WidgetReducerType<ChartState, ChartMessage> = (state = getInitialState(), message) => {
    // test for initialState calls
    if (!message) {
        return state
    }

    if (message.baseline && message.type !== 'event') {
        return state
    }

    switch (message.type) {
        case 'start':
            return getInitialState()

        case 'event':
            return {
                value: message.events[0].fieldA + message.events[0].fieldB
            }
        default:
            return state
    }
}
```
{% endcode %}

On the reducer above we're getting fields `fieldA` and `fieldB` defined in `ChartEvent` to consolidate our state value on data events. [All event types definitions](https://platform.intelie.com/developers/web-application/dashboard-and-widgets/live-event-types).

## Creating a pure Javascript Widget

### Widget implementation

Create a simple class that implements `ChartInterface`

{% code title="SimpleChart.ts" %}
```typescript
class SimpleChart implements ChartInterface<ChartState, ChartMessage, Configs> {}
```
{% endcode %}

After this, let the IDE or Code editor help you:

![Automatically implement ChartInterface](<../../../.gitbook/assets/image (24) (1).png>)

You should get something like this:

{% code title="SimpleChart.ts" %}
```typescript
class SimpleChart implements ChartInterface<ChartState, MyMessage, ChartConfigs> {
    // Additionally: create custom instance properties
    cfg: ChartConfigs
    el: HTMLElement

    // Additionally: use the constructor to get needed values
    constructor({ options, el }: ChartConstructorOptions<ChartState, FlowMessage, ChartConfigs>) {
        this.cfg = options.cfg
        this.el = el
    }

    init(): void {
        this.el.innerHTML = 'Chart has been initialized'
    }

    render(state: ChartState, configs: Configs, size: Dimensions): void {
        this.el.innerHTML = `value is ${state.value}, size is ${size.width}x${size.height}`
        this.el.style.color = configs.widget.jsonConfig.color
    }

    destroy(): void {
        this.el.remove()
    }
}
```
{% endcode %}

That is all on the chart implementation.&#x20;

### Registering the widget

Wrap your widget with the  `widgetWrapper` decorator

{% code title="init.ts" %}
```javascript
import widgetService from 'live/services/widgets'
import { widgetWrapper } from 'live/widgets/decorators/pure'
import SimpleChart from './SimpleChart'


widgetService.register({
    type: 'SimpleChart',
    get name() {
        return i18n('Simple chart')
    },
    get description() {
        return i18n('This chart shows the number 42')
    },
    config: {
        color: 'green'
    },
    // the wrapper is needed to enable the reducer mechanism and comply the API
    impl: widgetWrapper<ChartState, ChartMessage, ChartConfigs>(SimpleChart, simpleReducer)
})
```
{% endcode %}

You are all set. A new chart implementation has been added to Live and can now be chosen on the Pipes widget editor.&#x20;

### Complete example

{% tabs %}
{% tab title="SimpleChart.ts" %}
```typescript
class SimpleChart implements ChartInterface<ChartState, MyMessage, ChartConfigs> {
    // Additionally: create custom instance properties
    cfg: ChartConfigs
    el: HTMLElement

    // Additionally: use the constructor to get needed values
    constructor({ options, el }: ChartConstructorOptions<ChartState, FlowMessage, ChartConfigs>) {
        this.cfg = options.cfg
        this.el = el
    }

    init(): void {
        this.el.innerHTML = 'Chart has been initialized'
    }

    render(state: ChartState, configs: Configs, size: Dimensions): void {
        this.el.innerHTML = `value is ${state.value}, size is ${size.width}x${size.height}`
        this.el.style.color = configs.widget.jsonConfig.color
    }

    destroy(): void {
        this.el.remove()
    }
}
```
{% endtab %}

{% tab title="init.ts" %}
```typescript
import widgetService from 'live/services/widgets'
import { widgetWrapper } from 'live/widgets/decorators/pure'
import SimpleChart from './SimpleChart'
import { reducer } from './reducer'

widgetService.register({
    type: 'SimpleChart',
    get name() {
        return i18n('Simple chart')
    },
    get description() {
        return i18n('This chart shows the number 42')
    },
    config: {
        color: 'green'
    },
    // the wrapper is needed to enable the reducer mechanism and comply the API
    impl: widgetWrapper<ChartState, ChartMessage, ChartConfigs>(SimpleChart, simpleReducer)
})
```
{% endtab %}

{% tab title="types.ts" %}
```typescript
// 1. Typing the widget State shape:
// State will be the consolidated value derived
// from the events flow by the event reducer
// on this example we'll keep this simple shape:
type ChartState = {
    value: number
}

// 2. Typing data events:

// a. Event shape
type ChartEvent = LiveEvent<{
    fieldA: number
    fieldB: number
}>

// b. Describe your message shape
type ChartMessage = FlowMessage<ChartEvent>
 
 
// Typing the widget configuration:
type Configs = {
    widget: LiveWidget<{ color: string }>
}
```
{% endtab %}

{% tab title="reducer.ts" %}
```typescript
export const reducer: WidgetReducerType<ChartState, ChartMessage> = (state = getInitialState(), message) => {
    // test for initialState calls
    if (!message) {
        return state
    }

    if (message.baseline && message.type !== 'event') {
        return state
    }

    switch (message.type) {
        case 'start':
            return getInitialState()

        case 'event':
            return {
                value: message.events[0].fieldA + message.events[0].fieldB
            }
        default:
            return state
    }
}
```
{% endtab %}
{% endtabs %}

## Creating a React widget

### Widget implementation

To create a React widget simply create a React component, like so:

```typescript
// SimpleChart.tsx
const SimpleChart = (props: LiveReactChartProps<ChartState, ChartMessage, Configs>): JSX.Element => {
    const { state, width, height, widget } = props
    const { value } = state

    return (
        <div style={{ color: widget.jsonConfig.color }}>
            value is {value}, size is {width}x{height}
        </div>
    )
}
```

{% hint style="warning" %}
**React widget heads up**

If your component is a [**Memo**](https://reactjs.org/docs/react-api.html#reactmemo) remember to return a new state at the reducer when you want it to be updated, a shallow copy is enough.
{% endhint %}

{% hint style="info" %}
#### React widget tip: split data and config components

By doing this you can restrict React updates to state and configs in different sub-trees.

eg:  \<ChartData data={state} /> and \<ChartConfig config={widget.jsonConfig> width={width} height={height} />
{% endhint %}

### Registering the widget

Wrap your widget with the  `reactWidgetWrapper` decorator

```javascript
// init.ts
import widgetService from 'live/services/widgets'
import { reactWidgetWrapper } from 'live/widgets/decorators/react'

widgetService.register({
    type: 'SimpleChart',
    get name() {
        return i18n('Simple chart')
    },
    get description() {
        return i18n('This chart shows the number 42')
    },
    config: {
        color: 'green'
    },
    // the wrapper is needed to enable the reducer mechanism and comply the API
    impl: reactWidgetWrapper<ChartState, ChartMessage, ChartConfigs>(SimpleChart, simpleReducer)
})
```

You are all set. A new chart implementation has been added to Live and can now be chosen on the Pipes widget editor.

### Complete example

{% tabs %}
{% tab title="SimpleChart.ts" %}
```typescript
// SimpleChart.tsx
const SimpleChart = (props: LiveReactChartProps<ChartState, ChartMessage, Configs>): JSX.Element => {
    const { state, width, height, widget } = props
    const { value } = state

    return (
        <div style={{ color: widget.jsonConfig.color }}>
            value is {value}, size is {width}x{height}
        </div>
    )
}
```
{% endtab %}

{% tab title="init.ts" %}
```typescript
import widgetService from 'live/services/widgets'
import SimpleChart from './SimpleChart'
import { reducer } from './reducer'
import { reactWidgetWrapper } from 'live/widgets/decorators/react'

widgetService.register({
    type: 'SimpleChart',
    get name() {
        return i18n('Simple chart')
    },
    get description() {
        return i18n('This chart shows the number 42')
    },
    config: {
        color: 'green'
    },
    // the wrapper is needed to enable the reducer mechanism and comply the API
    impl: reactWidgetWrapper<ChartState, ChartMessage, ChartConfigs>(SimpleChart, simpleReducer)
})
```
{% endtab %}

{% tab title="types.ts" %}
```typescript
// 1. Typing the widget State shape:
// State will be the consolidated value derived
// from the events flow by the event reducer
// on this example we'll keep this simple shape:
type ChartState = {
    value: number
}

// 2. Typing data events:

// a. Event shape
type ChartEvent = LiveEvent<{
    fieldA: number
    fieldB: number
}>

// b. Describe your message shape
type ChartMessage = FlowMessage<ChartEvent>
 
 
// Typing the widget configuration:
type Configs = {
    widget: LiveWidget<{ color: string }>
}
```
{% endtab %}

{% tab title="reducer.ts" %}
```typescript
export const reducer: WidgetReducerType<ChartState, ChartMessage> = (state = getInitialState(), message) => {
    // test for initialState calls
    if (!message) {
        return state
    }

    if (message.baseline && message.type !== 'event') {
        return state
    }

    switch (message.type) {
        case 'start':
            return getInitialState()

        case 'event':
            return {
                value: message.events[0].fieldA + message.events[0].fieldB
            }
        default:
            return state
    }
}
```
{% endtab %}
{% endtabs %}

##

## Legacy API - deprecated

{% hint style="warning" %}
Deprecated since [Live 2.27.0](https://platform.intelie.com/release-notes#2-27-0-jul-2-2020), this became an underneath API that shouldn't be used directly anymore. Prefer the [**Standard API** ](live-widget-api.md#standard-api)to create new charts.

Live prior to 2.27 continues to support Legacy API as default API for chart creation. Some other helpers will be dropped soon also. For example, **live/lib/react-widget** is deprecated in favor of  **live/widgets/decorators/react**.
{% endhint %}

{% code title="app.js" %}
```javascript
import i18n from 'live/services/i18n'
import WidgetsService from 'live/services/widgets'

// The widget implementation
import TutorialWidget from './widget'

// required:
// registering widget name and description
WidgetsService.name('tutorial', i18n('tutorial widget title'))
WidgetsService.description('tutorial', i18n('tutorial widget description'))

// required:
// registering optional its default config, it also acts a config mask
// you should put every possible configuration here.
WidgetsService.config('tutorial', { textColor: 'green' })

// required:
// registering the implementation
WidgetsService.impl('tutorial', TutorialWidget)

// optional:
// configurations that changes the behavior
// of the default editor and also the widget rendering
WidgetsService.policies('tutorial', {})
WidgetsService.properties('tutorial', {})
```
{% endcode %}

Live provides a factory function in `live/widgets/helpers`to proper setup the prototype to use a function constructor for the Widget instance creation.&#x20;

Live is implemented using ReactJS, however the Live Widget API doesn't require widgets developers to write a React Component. The Widget implementation must provide the following methods:

{% code title="widget.js" %}
```javascript
import moment from 'moment'
import { extend } from 'lodash'
import { widgetFactory } from 'live/widgets/helpers'

const TutorialWidget = widgetFactory()

extend(TutorialWidget.prototype, {

    initialize() { }, // optional method
    destroy() { }, // optional method
    
    setEl(el) { return this },
    render() { return this },

    flow(type, events, layer, baseline, preventRedraw) {  },
})

module.exports = TutorialWidget
```
{% endcode %}

### Widget.setEl&#x20;

**setEl(element)** is called right after the DOM element creation for the widget container. The `element`parameter contains the `div`that acts as container and the developer could save the `element`reference at its instance for future usage. This method should return the current widget instance.

```typescript
setEl(el: HTMLElement) {
    this.el = el

    return this
}
```

### **Widget.render**

**render()** is called once at very first rendering of the Widget (be careful to avoid misunderstanding with render behavior of the React Components). This method is executed as a initialization phase at Widget Life-Cycle, it is commonly used to get the DOM ready for the events arrival. Besides the `element`container also receives a CSS class named using the `-widget` suffix over the widget type string, in our example: `tutorial-widget`. So the developer will be able to customize the container as necessary and append child elements as well, for example:

```javascript
render() {
    this.el.insertAdjacentHTML('beforeend', '<span class="value"> - </span>')
    this.el.insertAdjacentHTML('beforeend', '<span class="date"> - </span>')
    this.el.insertAdjacentHTML('beforeend', '<span class="control">waiting</span>')

    // Widget implementation MUST call the `onReady` function.
    if (typeof this.options.onReady == "function")
        this.options.onReady.apply(this)

    return this
}
```

### The \`render\` return types

#### Object with defined \`options\` property&#x20;

```javascript
{ options: LiveWidgetImplOptions }
```

If the render function returns a object with `options` it tells Live that the widget **handles its own rendering process**.  Then you must handle by your own any dynamic change to your own properties. Of course, in event flow method (as we describe next) you can handle how to change anything in your widget since new events arrive.

The `widgetFactory` constructor assigns a default empty object to `options`. Keep that in mind if your render method returns `this`.&#x20;

The`LiveWidgetImplOptions` type is described in [Live Widget Configuration](live-widget-configuration.md) section.

#### Function: considered to be React Component

If the render function returns a `Function`,  the return value is considered to be a React Component. Keep in mind this function definition **must not** define `options` property. The component will be created like so:

```javascript
<WidgetComponent
    mode={this.props.mode} // the dashboard mode or mode 'editor'
    event={this.state.event} // the last event
    batch={this.state.batch} // the last event batch
    widget={widget} // the widget configuration
    height={this.state.height} // current height
    width={this.state.width} // current width
/>
```

#### Object with \`portal\` property: property considered to be a React Component

The portal property is also considered to be a **React Component.** It differs from returning a function because this **PortalComponent** will be rendered as a sibling of the widget's HTMLElement reference element.\


### The \`onReady\` function

The `onReady` function received on the Widget's constructor and assigned to `this.options.onReady` is a function that as soon as it is called it flags the widget as _ready to receive data_. That said, **calling `this.options.onReady` is mandatory.**

### **Widget.flow**

**flow(type, events, layer, baseline, preventRedraw)** is called as soon as `onReady` Promise is resolved and will be executed every time the new data event or event control arrives. In strict sense of a Pipes Widgets, this is the most important method in widget implementation. It should be faster, be careful to avoid memory consumption and any delays on it will result in lower event processing throughput.

The event `type` string describes what kind of `events` is coming. Live organizes them in two categories `data events`and`control events`. For further understanding what event exists and how they like, see [Live Events Types](live-event-types.md) section.

The batch of `events` is an array of objects and its format depends on Pipes Expression (see **start** event at [Live Event Types](live-event-types.md)). If only single events are emitted each time, `events.length` will be always 1. But Pipes has support to emit at every N items collected, so the event could arrive in batch mode.&#x20;

Live Widget could have separated `layers`to execute different Pipes queries at the same time (the widget configuration could choose if multiple layers are supported). The `layers`parameter is a number with the layer index. The `baseline` and `preventRedraw` parameters are boolean.

An illustrative example how implement the flow function is given:

```javascript
flow(type, events, layer, baseline, preventRedraw) {
    switch (type) {
        // data events
        case 'event':
            const liveevent = events[0] // we take only one but it could be a batch
            const timestamp = liveevent.timestamp / 1000
            const event2str = JSON.stringify(liveevent, null, 2)
            const eventdate = 'event emitted in ' + moment.unix(timestamp).toString()
            this.el.querySelector('.value').innerHTML = event2str
            this.el.querySelector('.date').innerHTML = eventdate
            break
        // control events
        case 'start':
            this.el.querySelector('.control').innerHTML = 'starting...'
            break
        case 'warning':
            this.el.querySelector('.control').innerHTML = events.message
            break
        case 'endHistory':
            this.el.querySelector('.control').innerHTML = 'query duration was ' + events.duration + 'ms !'
            break
        default:
            break
    }
    // debugging only event controls
    if (type !== 'event')
        console.debug('event control "' + type + '" has arrived ', events)
}
```

The initialize and destroy methods are optional. Initialize is called at instantiation time and destroy is the last call before the Live removes the container from DOM.

The screenshot below shown an example of a chart instance of Tutorial Pipes Widget using the Pipes Expression `=> random() every 5 sec` that produces only real-time events that spawn every 5 seconds.

![Example of the tutorial widget being used at creation of a new Pipes Chart at Dashboard Edit Mode](<../../../.gitbook/assets/image (138).png>)

### Widget Life Cycle

Live Widget instance is created at very first time when the user click in widget name at side bar of widget editor (as shown above). But at this point the widget isn't part of a dashboard yet, the user must save it. After saved, now the widget gains an unique ID in dashboard context and will be instantiate every time the dashboard be open (either in edit mode or view mode). Internally a Live Widget instance is named Chart.

Some basic examples of properties of a widget instance are shown below:

* `this.options.id` : number that identifies the widget in dashboard
* `this.options.name` : string that identifies the widget name in dashboard
* `this.options.mode` : string that indicates one of:&#x20;
*
  * `editor`when widget itself is being edited
  * `edit` when dashboard is being edited
  * `view`when dashboard is opened in view mode
  * `fullscreen`when dashboard is opened in fullscreen mode

The [Live Widget Configuration](live-widget-configuration.md) section presents the complete reference of the widget configuration type.

### Appendix: complete example files

See the complete example files discussed in the previous headings. You could create a `webapp`directory, put all the following files on it and read the [Live Widget Packaging](live-widget-packaging.md) section to learn how to build a single bundle.js using Webpack.

{% tabs %}
{% tab title="js/app.js" %}
```javascript
import i18n from 'live/services/i18n'
import WidgetsService from 'live/services/widgets'
import TutorialWidget from './widget'

try {
    document.addEventListener('DOMContentLoaded', function () {
        require('./init')
    })
    
    // required:
    // registering widget name and description
    WidgetsService.name('tutorial', i18n('tutorial widget title'))
    WidgetsService.description('tutorial', i18n('tutorial widget description'))
    
    // required:
    // registering optional its default config, it also acts a config mask
    // you should put every possible configuration here.
    WidgetsService.config('tutorial', { textColor: 'green' })
    
    // required:
    // registering the implementation
    WidgetsService.impl('tutorial', TutorialWidget)
} catch (e) {
    console.error(e)
}

require('../scss/index.scss')
```
{% endtab %}

{% tab title="js/widget.js" %}
```javascript
import moment from 'moment'
import { extend } from 'lodash'
import { widgetFactory } from 'live/widgets/helpers'

const TutorialWidget = widgetFactory()

extend(TutorialWidget.prototype, {

    initialize() { },

    destroy() {
        console.log('widget.destroy() is called before DOM element removal')
        while (this.el.firstChild && !this.el.firstChild.remove())
        this.el.innerHtml = ''
    },

    render() {
        console.debug('widget.render() is called only at first widget rendering')
        this.el.insertAdjacentHTML('beforeend', '<span class="value"> - </span>')
        this.el.insertAdjacentHTML('beforeend', '<span class="date"> - </span>')
        this.el.insertAdjacentHTML('beforeend', '<span class="control">waiting</span>')

        // Widget implementation MUST call the `onReady` function.
        if (typeof this.options.onReady == "function")
            this.options.onReady.apply(this)

        return this
    },

    setEl(el) {
        this.el = el

        return this
    },

    flow(type, events, layer, baseline, preventRedraw) {
        switch (type) {
            // data events
            case 'event':
                const liveevent = events[0] // we take only one but it could be a batch
                const timestamp = liveevent.timestamp / 1000
                const event2str = JSON.stringify(liveevent, null, 2)
                const eventdate = 'event emitted in ' + moment.unix(timestamp).toString()
                this.el.querySelector('.value').innerHTML = event2str
                this.el.querySelector('.date').innerHTML = eventdate
                break
            // control events
            case 'start':
                this.el.querySelector('.control').innerHTML = 'starting...'
                break
            case 'warning':
                this.el.querySelector('.control').innerHTML = events.message
                break
            case 'endHistory':
                this.el.querySelector('.control').innerHTML = 'query duration was ' + events.duration + 'ms !'
                break
            default:
                break
        }
        // debugging only event controls
        if (type !== 'event')
            console.debug('event control "' + type + '" has arrived ', events)
    }

})

module.exports = TutorialWidget
```
{% endtab %}

{% tab title="js/init.js" %}
```javascript
try {
    var i18n = require('live/services/i18n')
    var pt_br = require('./i18n/pt_br').default
    var en_us = require('./i18n/en_us').default

    var locale = Live.settings && Live.settings.locale 
                    ? Live.settings.locale : 'pt_br'
    var localeFiles = {
        pt_br: pt_br,
        en_us: en_us
    }

    i18n.add(localeFiles[locale])
} catch (e) {
    console.error('Init.js in plugin-tutorial-widgets failed: ' + e)
}
```
{% endtab %}

{% tab title="scss/index.scss" %}
```css
.tutorial-widget {
  display: flex;
  flex-direction: column;
  
  .value {
    display: flex;
    flex: 1;
    align-items: center;
    justify-content: center;
    font-size : 20px;
    line-height : 1em;
    color: red;
  }
  
  .date {
    display: flex;
    justify-content: flex-end;
    font-size : 14px;
    text-align : right;
    line-height : 2em;
    color: gray;
  }

  .control {
    display: flex;
    justify-content: flex-end;
    font-size : 14px;
    text-align : left;
    line-height : 2em;
    color: gray;
  }
}
```
{% endtab %}

{% tab title="js/i18n/pt_br.js" %}
```javascript
export default {
    values: {
        'tutorial widget title': 'Tutorial',
        'tutorial widget title options': 'Opções para widget Tutorial',
        'tutorial widget description': 
            'Um widget simples para exercitar a extensibilidade do Live',
    }
}

```
{% endtab %}

{% tab title="js/i18n/en_us.js" %}
```javascript
export default {
    values: {
        'tutorial widget title': 'Tutorial',
        'tutorial widget title options': 'Tutorial widget options',
        'tutorial widget description': 
            'A simple widget to exercise Live extensibility features',
    }
}
```
{% endtab %}
{% endtabs %}
