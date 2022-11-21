---
description: >-
  HTML/JSX widgets is a type of widgets that enables the creation of any kind of
  visualization.
---

# JSX Widgets

![](<../../.gitbook/assets/image (71).png>)

The **JSX Markup** is a javascript module definition. This module can evaluate or export  the desired value.\
\
It accepts a [React.Node](https://reactjs.org/docs/react-component.html#render) or a **React Component** as return value. \
\
The Widget [props](https://reactjs.org/docs/components-and-props.html):

```
type Props = {
  mode: 'view' | 'edit' | 'editor', // dashboard's view/edit mode or editor
  event: Object, // the last event
  batch: Object[], // the last event batch
  widget: Object, // the saved widget configuration
  height: number, // the current height of the widget
  width: number, // the current width of the widget
}
```

**Simple example**

```
// query
=> random() every min

// JSX Markup
export default function MyWidget (props) {
	return <pre>{JSON.stringify(props, null, 2)}</pre>
}
```

**Recommended Boilerplate**

There is a recommended structure provided by the editor, it has event handling and rendering optimizations.

![](<../../.gitbook/assets/image (78).png>)

See [https://platform.intelie.com/developers/exposed-javascript-modules](https://platform.intelie.com/developers/exposed-javascript-modules) to check which libraries are available to use on JSX Widget code.
