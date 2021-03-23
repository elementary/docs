---
description: 'Adding Badges, Progress Bars, and Quick Lists'
---

# Launchers

Applications can show additional information in the dock as well as the application menu. This makes the application feel more integrated into the system and give user it's status at a glance. See [HIG for Dock integration](https://elementary.io/docs/human-interface-guidelines#dock-integration) for what you should do and what you shouldn't.

For this integration you can use the [Granite.Services.Application API](https://valadoc.org/granite/Granite.Services.Application.html). Since it uses the same D-Bus path as the [Unity Launcher API](https://valadoc.org/unity/Unity.LauncherEntry.html), the API can work accross many different distributions as it is widely supported by third party applications.

### Current API support:

| Service | Badge Counter | Progress Bar | Static Quicklist |
| :--- | :--- | :--- | :--- |
| Application menu | Yes | No | Yes |
| Dock | Yes | Yes | Yes |

## Setting Up

Before writing any code, you must add the library Granite to your build system. We already installed this library during [The Basic Setup](../writing-apps/the-basic-setup.md) when we installed `elementary-sdk`. Open your meson.build file and add the new dependency to the `executable` method.

```text
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

Your app must also be a `Gtk.Application` with a correctly set `application_id` as we previously set up in Hello World.

Though we haven't made any changes to our source code yet, change into your build directory and run `ninja` to build your project. It should still build without any errors. If you do encounter errors, double check your changes and resolve them before continuing.

Once you've set up `granite` in your build system and created a new `Gtk.Application` with an `application_id`, it's time to write some code.

## Badges

Showing a badge in the dock and Applications Menu with the number `12` is as easy as:

```text
Granite.Services.Application.set_badge_visible.begin (true);
Granite.Services.Application.set_badge.begin (12);
```

Keep in mind you have to set the `set_badge_visible` property to true, and use an int64 type for the `set_badge` property. The suffix `.begin` is required here since these are asynchronous methods.

## Progress Bars

The same goes for showing a progress bar, here we show a progress bar showing 20% progress:

```text
Granite.Services.Application.set_progress_visible.begin (true);
Granite.Services.Application.set_progress.begin (0.2f);
```

As you can see, the method `set_progress` takes a `double` value type and is a range between `0` and `1`: from 0% to 100%. As with badges, Don't forget that `set_progress_visible` must be `true` and `.begin` is required for asynchronous methods.

## Static Quicklists

Static quicklists do not involve writing any code or using any external dependencies.

Static quicklists are stored within your `.desktop` file. These are so called "actions". You can define many actions in your desktop file that will always show as an action in the application menu as well as in the dock.

The format is as follows:

```text
[Desktop Action ActionID]
Name=The name of the action
Icon=The icon of the action (optional)
Exec=The path to application executable and it's command line arguments (optional)
```

Let's take a look at an example of an action that will open a new window of your application:

```text
[Desktop Entry]
Name=Application name
Exec=application-executable
...

[Desktop Action NewWindow]
Name=New Window
Exec=application-executable -n
```

Note that adding `-n` or any other argument will not make your application magically open a new window. It is up to your application to handle and interpret command line arguments. The [GLib.Application API](https://valadoc.org/gio-2.0/GLib.Application.html) provides many examples and an extensive documentation on how to handle these arguments, particularly the [command\_line signal](https://valadoc.org/gio-2.0/GLib.Application.command_line.html).

Please take a look at a [freedesktop.org Additional applications actions section](https://standards.freedesktop.org/desktop-entry-spec/latest/ar01s10.html) for a detailed description of what keys are supported and what they do.

