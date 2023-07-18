---
description: >-
  Create custom editors for widgets, add the editor to the new widget menu or
  customize the menu
---

# Custom widget editors

When a new widget type is created using the [Widget API](live-widget-api.md), it will use the standard pipes widget editor by default. However, the developer may register a custom editor to be used for a widget type.

Here we will guide on how to do some of the most common tasks in the creation of a new widget using examples. For a detailed documentation of the available API please look at the JSDocs in the source code available at `live/services/editor-menu`.

![The "New chart" menu](<../../../.gitbook/assets/image (4) (1) (1).png>)

## Creating a new widget editor

To create a widget editor, there are three main things a developer should do: create an editor component, register it using the EditorService and register an item in the "New chart" menu in the dashboard using the EditorMenuService.

### Creating an editor component

The editor component receives as props the data (a`LiveWidget` object), the stores and actions and the `meta` array.

```javascript
import { ConfigBar, EditorContainer } from 'plugin-assets/shared/widget/components/styled'
import Configs from './Configs'
const MySimpleEditor = (props: ChannelsWidgetEditorProps)=> {
    return (<ThemedEditorContainer className="editor-container default-editor">
        <ConfigBar>
            <Configs
                widgetConfig={this.props.data.jsonConfig}
                type={this.props.selectedWidget.type}
                changeConfig={this.props.widgetConfigActions.changeConfig}
                changeLayoutConfig={this.props.widgetConfigActions.changeLayoutConfig}
                isEditor={true}
            />
        </ConfigBar>
        <PreviewAndData
            widgetConfigStore={this.props.widgetConfigStore}
            widgetConfigActions={this.props.widgetConfigActions}
            widgetDataStore={this.props.widgetDataStore}
            widgetDataActions={this.props.widgetDataActions}
            widget={this.props.data}
            renderPreview={this.props.renderPreview}
            isEmpty={isEmpty}
            isAssetSelected={selectedAsset}
        />
    </ThemedEditorContainer>)
}

```

For the editor to be registered, it must provide a Higher Order wrapper component that receives the store and actions as parameters, as well as the `meta` array.

```javascript
export default function MySimpleEditorHOC(
    widgetConfigStore: Record<string, any>,
    widgetConfigActions: Record<string, any>,
    widgetDataStore: Record<string, any>,
    widgetDataActions: Record<string, any>,
    meta: Array<any>
): (props: ChannelsValueTypeActionProps) => React.ReactNode {
    return function MySimpleEditorWrapper(props: ChannelsValueTypeActionProps): React.ReactNode {
        return (
            <MySimpleEditor
                widgetConfigStore={widgetConfigStore}
                widgetDataActions={widgetDataActions}
                widgetConfigActions={widgetConfigActions}
                widgetDataStore={widgetDataStore}
                meta={meta}
                {...props}
            />
        )
    }
}
```

### Registering the widget editor

Following the example from the [Widget API](live-widget-api.md), we will add an editor to the `'my-simple-chart'`  widget type:

```javascript
import EditorService from 'live/services/editor'
import MySimpleEditorHOC from './MySimpleEditorComponent'
const widgetType = 'my-simple-chart'
const editorType = 'my-simple-chart-editor'

EditorService.newEditor({
    name: editorType,
    editor: MySimpleEditorHOC,
    defaultWidgetType: widgetType
})
```

Once registered, the url `/#/widget/new?dashboard=123&type=my-simple-chart-editor`  will be available to create a widget (in the dashboard `123`, in this example). &#x20;

### Registering a menu option

For the widget editor to be accessible in the dashboard menu, it must be registered using the EditorMenuService API. Since 2.28.0 images (210px x 100px) are used to represent the editors instead of icons. Editors added using the API below will be added to the "Standard charts" tab. If you want to customize the tabs, use the method described in the next section.

Starting in Live 2.28.0 you can use the following API to add a widget editor to the "Standard chart" tab.

```javascript
import EditorMenuService from 'live/services/editor-menu'
import i18n from 'live/services/i18n'
import thumbnail from 'img/chart-editor/example.png'

const option = {
    order: 0,
    type: 'my-simple-chart',
    label: i18n('My Simple Chart'),
    description: i18('This chart is an example for the live documentation'),
    thumbnail: thumbnail
}

EditorMenuService.add(option)
```

In previous versions, the following API should be used.

```javascript
import EditorService from 'live/services/editor'
import i18n from 'live/services/i18n'

const option = {
    order: 0,
    type: 'my-simple-chart',
    label: i18n('My Simple Chart'),
    description: i18('This chart is an example for the live documentation'),
    iconClass: 'fa fa-bar-chart'
}

EditorService.newEditorMenuOption(option.type, option)
```

## Customizing the menus

While using the API described previously is recommended, it is possible to customize the tabs, creating new tabs, removing existing tabs and replacing the default "Standard Charts" tab. Notice internally we call tabs "groups" since the visual representation of these groups may change from tabs to some other representation.&#x20;

If there are no widget editors in the standard tab, it will not be shown in the interface nor listed in the API.

#### Replacing the standard tab

The developer may replace the standard tab to rename it or change the order of the tabs. All existing editors added to the standard tab will be moved to the new standard tab and new editors added using the default API will be added to the new standard tab as well.

```javascript
const newStandardGroup = new EditorMenuOptionGroup({ label: 'test', order: 100 })

EditorMenuService.replaceStandardGroup(newStandardGroup)
```

#### Adding a new tab

The developer may add a new tab to the menu and add editors to it

```javascript
const group = new EditorMenuOptionGroup({ order: 0, label: 'Example group' })

const editorMenuExample = {
    order: 1,
    type: 'example',
    label: 'Example chart',
    description: 'A chart that is used as an example for code testing purposes',
    thumbnail: 'https://www.intelie.com/images/id-intelie-new.png'
}

group.add(editorMenuExample)

EditorMenuService.add(group)
```

#### Removing a tab

The developer may remove a tab if he so wishes, but is responsible for adding the editors in that tab to some other tab

```javascript
const allOptions = EditorMenuService.options()

EditorMenuService.groups().forEach((group) =>
    const groupOptions = group.options()
    EditorMenuService.remove(group)
)

```
