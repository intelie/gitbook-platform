---
description: >-
  Live provides an API to register new menu items at navigation bar.
---

# Standard API

{% hint style="info" %}
Since Live @ 2
{% endhint %}

# Menu service

The menuService is available in live/services/menu. It is javascript module that allows registering custom menu items to Live navigation bar or an item that already exists at the navigation bar. Every menu item must have a unique alias, name and route URL to represent itself. In this tutorial example we choose `Collectors` as shown below:

## Methods

> register(menu: MenuItem, type: MenuType): void

Register a new menu item.

> unregister(menu: MenuItem, type: MenuType): void

Unregister an already existing menu item.

> getAll(): MenuItems

Obtain all registered menu items.

> withMenuItems(Component: React.ComponentType<{ menus: MenuItems }>): React.FC

Allow a React component to receive all listed menus.

## MenuItem

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
