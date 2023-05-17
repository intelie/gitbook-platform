# Registering Home Page options

{% hint style="info" %}
Since Live 3.32.0
{% endhint %}

Live's home page can be redefined in the admin "System and Display Settings" screen, under the "UI Configurations" section. To see more details about the configuration, see [Home Page Customization](../../administration/configuration/home-page.md)

Home Page options can be registered by other plugins using the `LiveApi.HomePages` registry in the frontend. A home page option can basically be one of three kinds:

- a redirect to a page that already exists on Live's menu, in which case the URL to that page must be provided when registering the home page option
- a page that renders a custom component, which must be provided when registering the home page option
- a redirect to a page of an instance of an entity (such as a specific dashboard or lookup table), in which case some endpoints and mapping functions related to the entity must be provided when registering the home page option

The `HomePage` type of a home page option is thus an union of the three types representing these kinds, defined below:

```typescript
// base interface with some common properties
interface HomePageBase {
    key: string
    label: string
    existsOnMenu: boolean
    permissions?: string[]
}

// renders custom component provided in the Page property
interface CustomComponentPage extends HomePageBase {
    existsOnMenu: false
    Page: React.ComponentType
}

// redirects to page that already exists on Live's menu
interface ExistingPage extends HomePageBase {
    existsOnMenu: true
    url: string
}

// redirects to existing page of specific instance of an entity
interface EntityPage extends HomePageBase {
    existsOnMenu: true
    entityData: {
        endpoint: string
        idToEndpoint: (id: string) => string
        resultsToLabelAndValue: (
            endpointResults: unknown
        ) => {
            label: string
            value: string
        }[]
        idToUrl: (id: string) => string
        hasPermission: (
            entity: unknown,
            successFetching: boolean,
            userPermissions: string[],
            userPerspectiveIds: string[] | 'all',
            error?: unknown
        ) => boolean
    }
}

// final union type
type HomePage = CustomComponentPage | ExistingPage | EntityPage
```

The parameters are explained in more detail in the table below:

| Parameter      | Description                                                                                                                                                                                                                          |
| -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `key`          | a unique key to identify this home page option                                                                                                                                                                                       |
| `label`        | a textual label representing this home page option, shown when selecting the option                                                                                                                                                  |
| `existsOnMenu` | a flag indicating whether the home page option is a page that already exists and can be accessed on Live's menu                                                                                                                      |
| `permissions`  | an optional list of permission names; to be able to access this home page option, the user must have at least one of them; the names should be in uppercase as registered by Live or plugins (e.g.: `DASHBOARDS`, `VIEW_MONITORING`) |
| `url`          | if the page already exists on Live's menu, the URL to it                                                                                                                                                                             |
| `Page`         | if the page does not exist on Live's menu, the page's React component                                                                                                                                                                |

The `entityData` parameter must be provided when registering an entity as the home page option (e.g.: dashboards). Its parameters are explained below:

| Parameter                | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                  |
| ------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `endpoint`               | the endpoint that should be called to get the list of all instances of the entity (e.g.: `/rest/dashboard` returns the data for all dashboards)                                                                                                                                                                                                                                                                                                              |
| `idToEndpoint`           | a function that receives an id and returns the endpoint that should be called to get the entity instance with that id                                                                                                                                                                                                                                                                                                                                        |
| `resultsToLabelAndValue` | a function that receives the results from calling the `endpoint` and formats them to a list of objects with a `label` (usually the name of the entity instance or some equivalent property) and `value` (the id of the instance or some equivalent property)                                                                                                                                                                                                 |
| `idToUrl`                | a function that receives an id and returns the URL of the page of the entity instance with that id                                                                                                                                                                                                                                                                                                                                                           |
| `hasPermission`          | a function that receives the object with an entity instance's data, along with a list with the names of the logged user's permissions and another with the ids of the perspectives they can access (or the string `all` when the user can access all of them), and returns a boolean value indicating whether they have permission to access that entity instance's page; the `successFetching` and `error` arguments are also provided to signal if there was an error when trying to fetch the entity instance's data |

### Examples

The first example below shows how to register an existing page as a home page option:

```javascript
import { LiveApi } from 'live/lib/api'

// register Live's 'Monitoring' page
LiveApi.HomePages.register({
    key: 'monitoring',
    label: 'Monitoring',
    existsOnMenu: true,
    url: '/#/monitoring',
    permissions: ['VIEW_MONITORING', 'MANAGE_MONITORING']
})
```

The next example shows how to register a new page as a home page option:

{% tabs %}
{% tab title="app.js" %}
```javascript
import { LiveApi } from 'live/lib/api'

import NewHomePage from './NewHomePage'

// register the new home page component imported
LiveApi.HomePages.register({
    key: 'new-home-page',
    label: 'New Home Page',
    existsOnMenu: false,
    Page: NewHomePage
})
```
{% endtab %}
{% tab title="NewHomePage.jsx" %}
```javascript
import React from 'react'

const NewHomePage = () => {
    <main style={{ backgroundColor: 'yellow' }}>
        <h1>New Home Page</h1>

        <p>
            Lorem ipsum dolor sit amet, consectetur adipiscing elit.
            Etiam eget ligula eu lectus lobortis condimentum.
            Aliquam nonummy auctor massa.
        </p>
    </main>
}

export default NewHomePage
```
{% endtab %}
{% endtabs %}

Finally, the last example shows how to register an entity (lookup tables in the example) as a home page option, which would allow a specific lookup table's page to be selected as a home page, as shown in the figure after the code.

```javascript
import { LiveApi } from 'live/lib/api'

LiveApi.HomePages.register({
    key: 'lookup',
    label: 'Lookup table',
    existsOnMenu: true,
    entityData: {
        endpoint: '/rest/lookup',
        idToEndpoint: id => `/rest/lookup/${id}`,
        resultsToLabelAndValue: data => (
            data?.data?.map(lookup => ({
                label: lookup.name,
                value: lookup.id
            }))
        ) || [],
        idToUrl: id => `/#/config/lookup-table/${id}`,
        hasPermission: () => true
    }
})
```

![Choosing a lookup table as a home page](<../../.gitbook/assets/home-page-customization-5.png>)
