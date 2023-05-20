---
description: Deep linking to System Settings
---

# System Settings

Some of the features of your application may depend on system level settings or services, such as Internet connectivity or app level access to the network. While your app cannot directly change those settings, it can prompt users to change them by linking to specific panes in the System Settings.

elementary OS implements a special `settings://` URI scheme for linking to System Settings. The links lead to specific settings panes. You can find most commonly used ones in the `Granite.SettingsUri` namespace in the Granite library.

## Opening a link

First, make sure you've included Granite in the build dependencies declared in your `meson.build` file:

{% code title="meson.build" %}
```coffeescript
executable(
    meson.project_name(),
    'src' / 'Application.vala',
    dependencies: [
        dependency('granite-7'),
        dependency('gtk4')
    ],
    install: true
)
```
{% endcode %}

Then you can open Network settings by launching `Granite.SettingsUri.NETWORK`:

```vala
var button = new Gtk.Button.with_label ("Change network settings");
button.clicked.connect (() => {
    try {
        AppInfo.launch_default_for_uri (Granite.SettingsUri.NETWORK, null);
    } catch (Error e) {
        warning ("Failed to open network settings: %s", e.message);
    }
});
```

You can find out more about the `launch_default_for_uri` function in [its documentation](https://valadoc.org/gio-2.0/GLib.AppInfo.launch_default_for_uri.html).

## Use cases

There are roughly two categories of settings that you can prompt users to change: general purpose and app specific. General ones include:

* `Granite.SettingsUri.NETWORK`, where connection to the Internet can be enabled
* `Granite.SettingsUri.ONLINE_ACCOUNTS`, where users can add their CalDAV and IMAP accounts
* `Granite.SettingsUri.SOUND_INPUT`, where microphone input can be enabled

while app specific ones are:

* `Granite.SettingsUri.PERMISSIONS`, where [Flatpak permissions](https://docs.flatpak.org/en/latest/sandbox-permissions.html#permissions-guidelines) are managed
* `Granite.SettingsUri.LOCATION`, where access to location data can be granted
* `Granite.SettingsUri.NOTIFICATIONS`, where presentation of app notifications can be customized
* `Granite.SettingsUri.SHORTCUTS`, where global keyboard shortcuts can be added

