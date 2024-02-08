---
description: >-
  Used to attach a new item at the navigation bar or a new item inside an option of the navigation bar (like "Preferences", "Apps", "Console").
---

# Menu service

{% hint style="info" %}
Since Live @ 2
{% endhint %}

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
    selectedAliases: string[]
    Component: React.Component
    name: string
    url: string
    replaces: string
    config: { showOnMobile: boolean }
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