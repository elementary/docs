# State Saving

Apps should automatically save their state in elementary OS, allowing a user to re-open a closed app and pick up right where they left off. To do so, we utilize `GSettings` via [`GLib.Settings`](https://valadoc.org/gio-2.0/GLib.Settings.html). GSettings allows your app to save certain stateful information in the form of booleans, strings, arrays, and more. It's a great solution for window size and position as well as whether certain modes are enabled or not. Note that GSettings is ideal for small amounts of configuration or stateful data, but user data \(i.e. documents\) should be stored on the disk.

## Using GSettings

For the simplest example, let's create a useless switch in your app, and save its state.

1. First, you need to define what settings your app will use so they can be accessed by your app. In your `data/` folder, create a new file named `gschema.xml`. Inside it, let's define a key for the useless switch:

   ```markup
   <?xml version="1.0" encoding="UTF-8"?>
   <schemalist>
     <schema path="/com/github/yourusername/yourrepositoryname/" id="com.github.yourusername.yourrepositoryname">
       <key name="useless-setting" type="b">
         <default>false</default>
         <summary>Useless Setting</summary>
         <description>Whether the useless switch is toggled</description>
       </key>
     </schema>
   </schemalist>
   ```

   The schema's `path` and `id` attributes are your app's ID \(`path` in a `/` format while `id` is the standard dot-separated format\). Note the key's `name` and `type` attributes: the name is a string to reference the setting, while in this case `type="b"` defines the setting as a boolean. The key's summary and description are developer-facing and are exposed in developer tools like dconf Editor.

2. Next, you need to be able to access `GLib.Settings` to be able to easily save and load settings. In your app's `Gtk.Application`, create a new instance of GLib.Settings for your app's ID:

   ```csharp
   var settings = new GLib.Settings ("com.github.yourusername.yourrepositoryname");
   ```

3. For our switch example, we can simply [bind](https://valadoc.org/gio-2.0/GLib.Settings.bind.html) the switch's `active` state to the GSetting. Beneath your settings definition, let's add a switch and the binding:

   ```csharp
   var useless_switch = new Gtk.Switch ();
   settings.bind ("useless-setting", useless_switch, "active", GLib.SettingsBindFlags.DEFAULT);
   ```

   You can read more about [`GLib.Settings.bind ()` on Valadoc](https://valadoc.org/gio-2.0/GLib.Settings.bind.html), but for now this will bind the `active` state of the switch to the value of `useless-setting` in GSettings. When one changes, the other will stay in sync to reflect it.

4. Be sure to attach your switch to your layout so it shows up.
5. We need to add the new GSchema XML file to our build system so it is included at install time. Open `data/meson.build` and type the following:

   ```text
   install_data (
       'gschema.xml',
       install_dir: join_paths (get_option ('datadir'), 'glib-2.0', 'schemas'),
       rename: meson.project_name () + '.gschema.xml'
   )
   ```

   This ensures your gschema.xml file is installed, and renames it with your app's ID to avoid filename conflicts.

6. Compile and install your app to see it in action! Note that GSettings are installed at install time, not compile time. So you'll need to run `ninja install` to avoid crashes from non-existent settings. To see the setting, you can open your app, toggle the switch, close it, then re-open it. The switch should retain its previous state. To see it under the hood, open dconf Editor, navigate to your apps settings at com.github.yourusername.yourrepositoryname, and watch the `useless-setting` when you toggle your switch.

