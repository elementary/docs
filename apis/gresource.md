# Custom Resources

You can include additional resources with your app like icons or CSS files using GResource. When using GResource, these files will be compiled into your app's binary, ensuring they're available and loaded when your app launches. Regardless of which type of resource you'd like to include, you'll need to create a `gresource.xml` file and include it in your build system. Make sure to start with a `Gtk.Application` as described in the [previous section](../writing-apps/our-first-app/the-build-system.md)

In the `data` directory, create a new file called `gresource.xml` with the following contents:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="/com/github/yourusername/yourrepositoryname">
  </gresource>
</gresources>
```

For now, this resource doesn't include any files. We'll come back to that in the following sections. Make sure to update the resource prefix to match your app's RDNN.

Now add the following lines to `meson.build`, somewhere before the `executable` step:

```coffeescript
# Include the GNOME module
gnome = import('gnome')

# Tell meson where to find our resources file and to compile it as a GResource
gresource = gnome.compile_resources(
    'gresource',
    'data' / 'gresource.xml',
    source_dir: 'data'
)
```

and finally, add the gresource to your `executable` step:

```coffeescript
executable(
    meson.project_name(),
    gresource,
    'src/Application.vala',
    dependencies: [
        dependency('gtk+-3.0')
    ],
    install: true
)
```

Now that we have a `gresource.xml` file and have included it in the build system, we can add files and reference them in our app.

## Icons

As we saw in the section on [`GLib.Action`](actions.md), GTK has a baked-in set of icon sizes defined under the namespace [`Gtk.IconSize`](https://valadoc.org/gtk+-3.0/Gtk.IconSize.html). When creating icons, it is important to know which of these sizes will be used and to design and hint the icon at that size. For more information about creating and hinting icons, check out the [Human Interface Guidelines](https://docs.elementary.io/hig/reference/iconography#size). Add your custom icon to the `data` directory, and then update your `gresource.xml` file to reference it:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="/com/github/yourusername/yourrepositoryname">
    <file compressed="true" preprocess="xml-stripblanks">custom-icon.svg</file>
  </gresource>
</gresources>
```

If you want to use the same icon name in multiple sizes in your app, you can `alias` the icon to paths in [hicolor](https://specifications.freedesktop.org/icon-theme-spec/latest/ar01s03.html) and GTK will automatically load the correct version when its size is referenced:

```markup
<?xml version="1.0" encoding="UTF-8"?>
<gresources>
  <gresource prefix="/com/github/yourusername/yourrepositoryname">
    <file alias="24x24/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-24.svg</file>
    <file alias="24x24@2/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-24.svg</file>
    <file alias="32x32/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-32.svg</file>
    <file alias="32x32@2/actions/custom-icon.svg" compressed="true" preprocess="xml-stripblanks">custom-icon-32.svg</file>
  </gresource>
</gresources>
```

Now we can add the GResource path to the system's built-in icon theme in the `Application` class' `activate ()` function. This will let us reference the icon name without using long paths, and automatically handle icon sizing as previously mentioned Again, don't forget to use the same RDNN'd path that was defined in `gresource.xml`:

```csharp
protected override void activate () {
    Gtk.IconTheme.get_default ().add_resource_path ("com/github/yourusername/yourrepositoryname");

    var main_window = new Gtk.ApplicationWindow (this);
    main_window.show_all ();
}
```

The last step is to create a `Gtk.Image` or `Gtk.Button` using your custom icon and add it to the main window:

```csharp
protected override void activate () {
    Gtk.IconTheme.get_default ().add_resource_path ("/com/github/yourusername/yourrepositoryname");

    // This will create an image with an icon size of 32px
    var image = new Gtk.Image.from_icon_name ("custom-icon", Gtk.IconSize.DND);

    // This will create a button with an icon size of 24px
    var button = new Gtk.Button.from_icon_name ("custom-icon", Gtk.IconSize.LARGE_TOOLBAR);

    var grid = new Gtk.Grid () {
        column_spacing = 12
    };
    grid.add (image);
    grid.add (button);

    var main_window = new Gtk.ApplicationWindow (this);
    main_window.show_all ();
}
```

