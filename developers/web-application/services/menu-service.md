---
description: >-
  Used to attach a new item at the navigation bar or a new item inside an option of the navigation bar (like "Preferences", "Apps", "Console").
---

# Menu service

{% hint style="info" %}
Since Live @ 2
{% endhint %}

## Menu Item

The properties of the menu item are explained in more detail in the table below:

| Property | Description |
| --------- | ----------- |
| `alias` | textual representation of the menu, used to apply the "selected" status to the option at the navbar |
| `order` | numeric representation of the order that the menu will appear |
| `selectedAliases` | array with all textual names the menu will have. if your route/url is different of the alias, you must put both in this array to apply the "selected" status when your menu option is selected |
| `Component` | the component that will be rendered when the menu is selected |
| `name` | textual name of the menu that will be displayed in the navbar |
| `url` | route to access the component of the menu |
| `replaces` | textual representation of an alias of another menu item you want to replace with the one you're creating |
| `config.showOnMobile` | if set to false, it will not show the menu option when Live is in mobile mode |

## Methods

> register(menu: MenuItem, type: MenuType): void

Register a new menu item.

> unregister(menu: MenuItem, type: MenuType): void

Unregister an already existing menu item.

> getAll(): MenuItems

Obtain all registered menu items.

> withMenuItems(Component: React.ComponentType<{ menus: MenuItems }>): React.FC

Allow a React component to receive all listed menus.

## Parameters

`menu` is the object that represents the menu you want to attach to the navigation bar.

`type` is an option that represents where it will be located. It can be at the `regular` location (like "Home", "Dashboards", "Configurations"), at the `right` side of the bar (like the "Messenger"), or inside an item that already exists (like "`console`", "`preferences`", or "`apps`").

`Component` Is the React component which will have the menus.

## Code

```typescript
type MenuType = 'right' | 'regular' | 'preferences' | 'apps' | 'console'

interface MenuItem {
    alias: string
    order: number
    selectedAliases?: string[]
    Component: React.Component
    name: string
    url: string
    replaces?: string
    config?: { showOnMobile: boolean }
}

interface MenuItems {
    [key: string]: MenuItem[]
}

const MENU_ITEMS: Record<MenuType, MenuItem[]> = observable({
    right: [],
    regular: [],
    preferences: [],
    apps: [],
    console: []
})

const MenuService = function () {
    return {
        register(menu: MenuItem, type: MenuType): void {
            const list = MENU_ITEMS[type]

            if (list) {
                list.push(menu)
                MENU_ITEMS[type] = sortBy(list, ['order', 'alias'])
            }
        },

        unregister(menu: MenuItem, type: MenuType): void {
            const list = MENU_ITEMS[type]

            if (list) {
                MENU_ITEMS[type] = list.filter(menuItem => menuItem.alias !== menu.alias)
            }
        },

        getAll(): MenuItems {
            return toJS(MENU_ITEMS)
        },

        withMenuItems(Component: React.ComponentType<{ menus: MenuItems }>): React.FC {
            return observer(function ComponentWithMenus() {
                const menus = toJS(MENU_ITEMS)

                return <Component menus={menus} />
            })
        }
    }
}

module.exports = new MenuService()
```