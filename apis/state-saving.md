# State Saving

Apps should automatically save their state in elementary OS, allowing a user to re-open a closed app and pick up right where they left off. To do so, we utilize `GSettings` via [`GLib.Settings`](https://valadoc.org/gio-2.0/GLib.Settings.html). GSettings allows your app to save certain stateful information in the form of booleans, strings, arrays, and more. It's a great solution for window size and position as well as whether certain modes are enabled or not. Note that GSettings is ideal for small amounts of configuration or stateful data, but user data \(i.e. documents\) should be stored on the disk.

For the simplest example, let's create a switch in your app, and save its state.

## Define Settings Keys

First, you need to define what settings your app will use so they can be accessed by your app. In your `data/` folder, create a new file named `gschema.xml`. Inside it, let's define a key for the switch:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<schemalist>
  <schema path="/com/github/yourusername/yourrepositoryname/" id="com.github.yourusername.yourrepositoryname">
    <key name="useless-setting" type="b">
      <default>false</default>
      <summary>Useless Setting</summary>
      <description>Whether the switch is toggled</description>
    </key>
  </schema>
</schemalist>
```

The schema's `path` and `id` attributes are your app's ID \(`path` in a `/` format while `id` is the standard dot-separated format\). Note the key's `name` and `type` attributes: the name is a string to reference the setting, while in this case `type="b"` defines the setting as a boolean. The key's summary and description are developer-facing and are exposed in developer tools like dconf Editor.

## Use The Settings Object

In order to interact with the settings key we just defined, we need to create a GLib.Settings object in our application. Building on previous examples, you can create a Gtk.Switch, add it to your window, create a GLib.Settings object, and bind the switch to a settings key like so:

```csharp
protected override void activate () {
    var switch = new Gtk.Switch () {
        halign = Gtk.Align.CENTER,
        valign = Gtk.Align.CENTER
    };

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300
    };
    main_window.add (switch);
    main_window.show_all ();

    var settings = new GLib.Settings ("com.github.yourusername.yourrepositoryname");
    settings.bind ("useless-setting", switch, "active", GLib.SettingsBindFlags.DEFAULT);
}
```

You can read more about [`GLib.Settings.bind ()` on Valadoc](https://valadoc.org/gio-2.0/GLib.Settings.bind.html), but for now this will bind the `active` property of the switch to the value of `useless-setting` in GSettings. When one changes, the other will stay in sync to reflect it.

## Install and Compile Schemas with Meson

We need to add the new GSchema XML file to our build system so it is included at install time. Create a file `data/meson.build` and type the following:

```text
install_data (
    'gschema.xml',
    install_dir: join_paths (get_option ('datadir'), 'glib-2.0', 'schemas'),
    rename: meson.project_name () + '.gschema.xml'
)
```

This ensures your gschema.xml file is installed, and renames it with your app's ID to avoid filename conflicts.

We'll also need to add a small Python script to tell Meson to compile our schemas. Create a new folder and a file `meson/post_install.py` that contains the following:

```python
#!/usr/bin/env python3

import os
import subprocess

schemadir = os.path.join(os.environ['MESON_INSTALL_PREFIX'], 'share', 'glib-2.0', 'schemas')

if not os.environ.get('DESTDIR'):
	print('Compiling gsettings schemas...')
	subprocess.call(['glib-compile-schemas', schemadir])
```

Be sure to add the following lines to the end of the meson.build file in your source root so that Meson knows where to find the additional instructions we've added:

```text
meson.add_install_script('meson/post_install.py')

subdir('data')
```

Compile and install your app to see it in action!

{% hint style="warning" %}
 GSettings are installed at install time, not compile time. So you'll need to run `ninja install` to avoid crashes from not-yet-installed settings.
{% endhint %}

To see the state saving functionality in action, you can open your app, toggle the switch, close it, then re-open it. The switch should retain its previous state. To see it under the hood, open dconf Editor, navigate to your app's settings at com.github.yourusername.yourrepositoryname, and watch the `useless-setting` update in real time when you toggle your switch.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/gsettings)
{% endhint %}
