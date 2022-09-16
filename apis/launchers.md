---
description: Adding Badges, Progress Bars, and Quick Lists
---

# Launchers

Applications can show additional information in the dock as well as the application menu. This makes the application feel more integrated into the system and give user it's status at a glance. See [HIG for Dock integration](https://docs.elementary.io/hig/widgets/providing-feedback#dock-integration) for what you should do and what you shouldn't.

For this integration you can use the [Granite.Services.Application API](https://valadoc.org/granite/Granite.Services.Application.html). Since it uses the same D-Bus path as the [Unity Launcher API](https://valadoc.org/unity/Unity.LauncherEntry.html), the API can work across many different distributions as it is widely supported by third party applications.

### Current API support:

| Service           | Badge Counter | Progress Bar | Actions |
| ----------------- | ------------- | ------------ | ------- |
| Applications Menu | ✔️ Yes        | ✖️ No        | ✔️ Yes  |
| Dock              | ✔️ Yes        | ✔️ Yes       | ✔️ Yes  |

## Setting Up

Before writing any code, you must add the library [Granite](https://valadoc.org/granite/Granite.html) to your build system. We already installed this library during [The Basic Setup](../writing-apps/the-basic-setup.md) when we installed `elementary-sdk`. Open your `meson.build` file and add the new dependency to the `executable` method.

```coffeescript
executable(
    meson.project_name(),
    'src/Application.vala',
    dependencies: [
        dependency('gtk+-3.0'),
        dependency('granite', version: '>=5.2.4') # 5.2.4 is the first release to support the Launcher API
    ],
    install: true
)
```

Your app must also be a `Gtk.Application` with a correctly set `application_id` as we previously set up in [Hello World](../writing-apps/hello-world.md#gtk.application).

Though we haven't made any changes to our source code yet, change into your build directory and run `ninja` to build your project. It should still build without any errors. If you do encounter errors, double check your changes and resolve them before continuing.

Once you've set up `granite` in your build system and created a new `Gtk.Application` with an `application_id`, it's time to write some code.

## Badges

Showing a badge in the dock and Applications Menu with the number `12` can be done with the following lines:

```vala
Granite.Services.Application.set_badge_visible.begin (true);
Granite.Services.Application.set_badge.begin (12);
```

Keep in mind you have to set the `set_badge_visible` property to true, and use an int64 type for the `set_badge` property. The suffix `.begin` is required here since these are asynchronous methods.

## Progress Bars

The same goes for showing a progress bar, here we show a progress bar showing 20% progress:

```vala
Granite.Services.Application.set_progress_visible.begin (true);
Granite.Services.Application.set_progress.begin (0.2f);
```

As you can see, the method `set_progress` takes a `double` value type and is a range between `0` and `1`: from 0% to 100%. As with badges, Don't forget that `set_progress_visible` must be `true` and `.begin` is required for asynchronous methods.

## Actions

Actions are specific functions your app can perform without already being open; think of them as alternate and more specific entry points into your app. Actions appear in the context menu of your app icon in the Applications Menu and Dock, and are searchable by name from the Applications Menu.

### D-Bus activation

Your app needs to support D-Bus activation in order to use actions as entry points. This does not require any changes to the application source code. All that is needed is a service file which is not unlike the `.desktop` file that you are already familiar with. Define a new `service.in` file in the `data` directory:

```ini
[D-BUS Service]
Name=com.github.myteam.myapp
Exec=@exec_name@ --gapplication-service
```

Notice the `@exec_name@` configurable variable and the `.in` file extension. Here we are using meson's [configuration feature](https://mesonbuild.com/Configuration.html) to specify the name of the executable. To install the service add this to your `meson.build` file:

```coffeescript
# Install D-Bus service, so that application can be started by D-Bus
service_conf_data = configuration_data()
service_conf_data.set('exec_name', get_option('prefix') / get_option('bindir') / meson.project_name())

configure_file(
    input: 'data' / 'service.in',
    output: meson.project_name() + '.service',
    configuration: service_conf_data,
    install: true,
    install_dir: get_option('datadir') / 'dbus-1' / 'services'
)
```

Lastly, update the `.desktop` file by adding the `DBusActivatable` line to the `Desktop Entry` group:

```ini
[Desktop Entry]
Name=Hello Again
[...]
DBusActivatable=true
```

### Declaring actions

You can use any action defined in the `app` namespace, i.e. registered with `GLib.Application`, as an entry point for your application. Implementing actions is covered in-depth in [the actions section](actions). They must also be declared in a new `Actions` line in your app's `.desktop` file. This line should contain a `;` separated list of action names:

```ini
[Desktop Entry]
Name=Hello Again
[...]
Actions=my-action;
```

Then use a dedicated group, named after the unique action name, to define the details of each action:

```ini
[Desktop Action my-action]
Name=My Great Action
Icon=com.github.myteam.myapp.my-action-icon
Exec=com.github.myteam.myapp
```

{% hint style="warning" %}
The action name used in `.desktop` file, both in Desktop Entry and later in Desktop Action groups, needs to match exactly the name used to register the action with `GLib.Applicatin` in the source code.
{% endhint %}

The `Icon` line is optional and should be an icon which represents the action that will be performed. The `Exec` line should be specified, but is used only for backwards compatibility in case your app ever runs in an environment without D-Bus activation support.

{% hint style="info" %}
The action name should not include your app's name, as it will always be displayed alongside your app. The action icon should also not be your app icon, as it may be shown in the menu for your app icon, or badged on top of the app icon.
{% endhint %}

See the [freedesktop.org Additional applications actions section](https://standards.freedesktop.org/desktop-entry-spec/latest/ar01s10.html) for a detailed description of what keys are supported and what they do.
