# Home Page Customization

{% hint style="info" %}
Since Live 3.32.0
{% endhint %}

Live's home page can be redefined in the admin "System and Display Settings" screen, under the "UI Configurations" section.

![Home Page configurations in the admin screen](<../../.gitbook/assets/home-page-customization-1.png>)

## Configuration

Administrators can create one or more rules to define the home page for the environment. Each of these rules targets one or more users and defines the home page shown for them. The target options can be the following:
- **User**: only the specified user
- **E-mail suffix**: all users whose e-mails end in the specified suffix
- **All users**: all users (including admins)
- **Team**: all users that are part of the specified team
- **Perspective**: all users that have access to the specified perspective
- **Permission**: all users that have the specified permission

![Home Page configuration rule target options](<../../.gitbook/assets/home-page-customization-2.png>)

{% hint style="info" %}
To learn more about teams, perspectives and permissions, check the [permissions](../../features/access-permision.md) page.
{% endhint %}

The home page options can be pages such as the default home page or the "Monitoring" page or specific pages in a category (such as a specific dashboard). The options are registered by Live itself or other plugins; for more details see the [Registering Home Page options](developers/web-application/home-page.md).

![Home Page configuration rule home page options](<../../.gitbook/assets/home-page-customization-3.png>)

### Order

In case more than one of the configuration rules are valid for the user, the first (from top to bottom) valid one is applied.

The order of the rules can be changed by dragging and dropping a rule, and as described above, it will affect the decision of which rule to apply.

![Changing Home Page configuration rules order with drag and drop](<../../.gitbook/assets/home-page-customization-4.png>)

### Defaults

When there are no configurations or none of the existing rules are valid for the user, they will access Live's default home page.

Also, if for some reason the custom home page fails to load for the user, they will also access Live's default home page.
