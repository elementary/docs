---
description: >-
  Include additional resources with your app like icons or CSS files using
  GResource
---

# Custom Resources

When using GResource, custom resource files will be compiled into your app's binary, ensuring they're available and loaded when your app launches. Regardless of which type of resource you'd like to include, you'll need to create a `gresource.xml` file and include it in your build system.

Make sure to start with a `Gtk.Application` as described in the [previous section](../writing-apps/our-first-app/the-build-system.md). In the `data` directory, create a new file called `gresource.xml` like the one below. Then update your meson build file to include steps to build the resource into your binary. Make sure to update the resource prefix to match your app's RDNN.

{% tabs %}
{% tab title="gresource.xml" %}
```xml
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="com/github/mteam/myapp">
  </gresource>
</gresources>
```
{% endtab %}

{% tab title="meson.build" %}
```coffeescript
# Include the GNOME module
gnome = import('gnome')

# Tell meson where to find our resources file and to compile it as a GResource
gresource = gnome.compile_resources(
    'gresource',
    'data' / 'gresource.xml',
    source_dir: 'data'
)

# Add the gresource to the executable step to be build into the app binary
executable(
    meson.project_name(),
    gresource,
    'src' / 'Application.vala',
    dependencies: [
        dependency('gtk4')
    ],
    install: true
)
```
{% endtab %}
{% endtabs %}

Now that we have a `gresource.xml` file and have included it in the build system, we can add files and reference them in our app.

## Icons

You can provide custom icons and have your app automatically refer to them by name and choose the correct size by adding them your Gresource file under the namespace `/icons`. For this to work properly, your resource path must match your application's ID.

Add a custom icon to the `data` directory, and then update your `gresource.xml` file to reference it:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="com/github/myteam/myapp/icons">
    <file compressed="true" preprocess="xml-stripblanks">custom-icon.svg</file>
  </gresource>
</gresources>
```

If you want to use the same icon name in multiple sizes in your app, you can `alias` the icon to paths in [hicolor](https://specifications.freedesktop.org/icon-theme-spec/latest/ar01s03.html) and GTK will automatically load the correct version when its size is referenced:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="com/github/myteam/myapp/icons">
    <file alias="24x24/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-24.svg</file>
    <file alias="24x24@2/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-24.svg</file>
    <file alias="32x32/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-32.svg</file>
    <file alias="32x32@2/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-32.svg</file>
  </gresource>
</gresources>
```

{% hint style="info" %}
When creating icons, it is important to know which sizes will be used and to design and hint the icon at that size. For more information about creating and hinting icons, check out the [Human Interface Guidelines](https://docs.elementary.io/hig/reference/iconography#size).
{% endhint %}

The last step is to create a Gtk.Image or Gtk.Button using your custom icon and add it to the main window:

```vala
protected override void activate () {
    // This will create an image with an icon size of 32px
    var image = new Gtk.Image.from_icon_name ("custom-icon") {
        pixel_size = 32
    };

    // This will create a button with an icon size of 16px
    var button = new Gtk.Button.from_icon_name ("custom-icon");

    var box = new Gtk.Box (Gtk.Orientation.HORIZONTAL, 12);
    box.append (image);
    box.append (button);

    var main_window = new Gtk.ApplicationWindow (this) {
        child = box
    };
    main_window.present ();
}
```
