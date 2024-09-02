# State Saving

Apps should automatically save their state in elementary OS, allowing a user to re-open a closed app and pick up right where they left off. To do so, we utilize `GSettings` via [`GLib.Settings`](https://valadoc.org/gio-2.0/GLib.Settings.html). GSettings allows your app to save certain stateful information in the form of booleans, strings, arrays, and more. It's a great solution for window size and position as well as whether certain modes are enabled or not. Note that GSettings is ideal for small amounts of configuration or stateful data, but user data (i.e. documents) should be stored on the disk.

For the simplest example, let's create a switch in your app, and save its state.

## Define Settings Keys

First, you need to define what settings your app will use so they can be accessed by your app. In your `data/` folder, create a new file named `gschema.xml`. Inside it, let's define a key for the switch:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<schemalist>
  <schema path="/io/github/yourusername/yourrepositoryname/" id="io.github.yourusername.yourrepositoryname">
    <key name="useless-setting" type="b">
      <default>false</default>
      <summary>Useless Setting</summary>
      <description>Whether the switch is toggled</description>
    </key>
  </schema>
</schemalist>
```

The schema's `path` and `id` attributes are your app's ID (`path` in a `/` format while `id` is the standard dot-separated format). Note the key's `name` and `type` attributes: the name is a string to reference the setting, while in this case `type="b"` defines the setting as a boolean. The key's summary and description are developer-facing and are exposed in developer tools like dconf Editor.

## Use The Settings Object

In order to interact with the settings key we just defined, we need to create a GLib.Settings object in our application. Building on previous examples, you can create a Gtk.Switch, add it to your window, create a Settings object, and bind the switch to a settings key like so:

```vala
protected override void activate () {
    var useless_switch = new Gtk.Switch () {
        halign = CENTER,
        valign = CENTER
    };

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        child = useless_switch
    };
    main_window.present ();

    var settings = new Settings ("io.github.yourusername.yourrepositoryname");
    settings.bind ("useless-setting", useless_switch, "active", DEFAULT);
}
```

You can read more about [`GLib.Settings.bind ()` on Valadoc](https://valadoc.org/gio-2.0/GLib.Settings.bind.html), but for now this will bind the `active` property of the switch to the value of `useless-setting` in GSettings. When one changes, the other will stay in sync to reflect it.

## Install and Compile Schemas with Meson

We need to add the new GSchema XML file to our build system so it is included at install time. Create a file `data/meson.build` and type the following:

{% code title="data/meson.build" lineNumbers="true" %}
```coffeescript
install_data (
    'gschema.xml',
    install_dir: get_option('datadir') / 'glib-2.0' / 'schemas',
    rename: meson.project_name() + '.gschema.xml'
)
```
{% endcode %}

This ensures your gschema.xml file is installed, and renames it with your app's ID to avoid filename conflicts.

Be sure to add the following lines to the end of the meson.build file in your source root so that Meson will compile the installed schemas:

```coffeescript
gnome = import('gnome')
gnome.post_install(glib_compile_schemas: true)

subdir('data')
```

Compile and install your app to see it in action!

{% hint style="warning" %}
GSettings are installed at install time, not compile time. So you'll need to run `ninja install` to avoid crashes from not-yet-installed settings.
{% endhint %}

To see the state saving functionality in action, you can open your app, toggle the switch, close it, then re-open it. The switch should retain its previous state. To see it under the hood, open dconf Editor, navigate to your app's settings at io.github.yourusername.yourrepositoryname, and watch the `useless-setting` update in real time when you toggle your switch.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/gsettings)
{% endhint %}
