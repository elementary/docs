---
description: Creating multi-pane app layouts
---

# Panes

So far, you've created apps with a single HeaderBar, but what about creating an app with a multi-pane layout like Mail or Tasks?

<figure><img src="../.gitbook/assets/Screenshot from 2023-06-05 13.47.51.png" alt=""><figcaption></figcaption></figure>

Before we begin, make sure you've included Granite in the build dependencies of your "meson.build" file. We'll be using it later for a couple of style classes.

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

\
Start with a new `Gtk.Application`, but this time set the `titlebar` property of the ApplicationWindow to an invisible grid. This will replace the default HeaderBar of the window since we'll be creating our own HeaderBars inside the paned layout.

```vala
var main_window = new Gtk.ApplicationWindow (this) {
    titlebar = new Gtk.Grid () { visible = false }
};
```

Then create a new [`Gtk.Paned`](https://valadoc.org/gtk4/Gtk.Paned.html) and set that as the child of the ApplicationWindow. A Paned is a resizable widget that can contain two children. There are various properties you can set to control the widgets behavior when the AppliactionWindow is resized; For the purposes of this tutorial we want the first pane to stay a fixed size and we don't want someone to be able to make the window smaller than the contents of both sides of the Paned.

```vala
var paned = new Gtk.Paned (Gtk.Orientation.HORIZONTAL) {
    resize_start_child = false,
    shrink_end_child = false,
    shrink_start_child = false
};

var main_window = new Gtk.ApplicationWindow (this) {
    child = paned,
    titlebar = new Gtk.Grid () { visible = false }
};
```

Next, let's create a new `Gtk.HeaderBar` and hide its title buttons. Then, we're going to pack our own [`Gtk.WindowControls`](https://valadoc.org/gtk4/Gtk.WindowControls.html) that only contain the buttons for one side of the Window. And finally, we'll put that HeaderBar in a Box and set that as the `start_child` of the Paned.

```vala
var start_header = new Gtk.HeaderBar () {
    show_title_buttons = false
};
start_header.add_css_class (Granite.STYLE_CLASS_FLAT);
start_header.pack_start (new Gtk.WindowControls (Gtk.PackType.START));

var start_box = new Gtk.Box (Gtk.Orientation.VERTICAL, 0);
start_box.append (start_header);

var paned = new Gtk.Paned (Gtk.Orientation.HORIZONTAL) {
    start_child = start_box,
    resize_start_child = false,
    shrink_end_child = false,
    shrink_start_child = false
};

var main_window = new Gtk.ApplicationWindow (this) {
    child = paned,
    titlebar = new Gtk.Grid () { visible = false }
};
```

Do the same thing again for the `end_child` of the Paned: create a HeaderBar with `END` WindowControls, inside a vertical Box.

```vala
var start_header = new Gtk.HeaderBar () {
    show_title_buttons = false
};
start_header.add_css_class (Granite.STYLE_CLASS_FLAT);
start_header.pack_start (new Gtk.WindowControls (Gtk.PackType.START));

var start_box = new Gtk.Box (Gtk.Orientation.VERTICAL, 0);
start_box.append (start_header);

var end_header = new Gtk.HeaderBar () {
    show_title_buttons = false
};
end_header.add_css_class (Granite.STYLE_CLASS_FLAT);
end_header.pack_end (new Gtk.WindowControls (Gtk.PackType.END));

var end_box = new Gtk.Box (Gtk.Orientation.VERTICAL, 0);
end_box.append (end_header);

var paned = new Gtk.Paned (Gtk.Orientation.HORIZONTAL) {
    start_child = start_box,
    end_child = end_box,
    resize_start_child = false,
    shrink_end_child = false,
    shrink_start_child = false
};

var main_window = new Gtk.ApplicationWindow (this) {
    child = paned,
    titlebar = new Gtk.Grid () { visible = false }
};
```

Finally, make a few small tweaks:

1. Set an empty label as the `title_widget` in the start header
2. Add `Granite.STYLE_CLASS_VIEW` to the end box
3. Set the default size and title properties of the ApplicationWindow

The final result should look like this:

{% code title="Application.vala" %}
```vala
public class MyApp : Gtk.Application {
    public MyApp () {
        Object (
            application_id: "io.github.myteam.myapp",
            flags: ApplicationFlags.FLAGS_NONE
        );
    }

    protected override void activate () {
        var start_header = new Gtk.HeaderBar () {
            show_title_buttons = false,
            title_widget = new Gtk.Label ("")
        };
        start_header.add_css_class (Granite.STYLE_CLASS_FLAT);
        start_header.pack_start (new Gtk.WindowControls (Gtk.PackType.START));

        var start_box = new Gtk.Box (Gtk.Orientation.VERTICAL, 0);
        start_box.append (start_header);

        var end_header = new Gtk.HeaderBar () {
            show_title_buttons = false
        };
        end_header.add_css_class (Granite.STYLE_CLASS_FLAT);
        end_header.pack_end (new Gtk.WindowControls (Gtk.PackType.END));

        var end_box = new Gtk.Box (Gtk.Orientation.VERTICAL, 0);
        end_box.add_css_class (Granite.STYLE_CLASS_VIEW);
        end_box.append (end_header);

        var paned = new Gtk.Paned (Gtk.Orientation.HORIZONTAL) {
            start_child = start_box,
            end_child = end_box,
            resize_start_child = false,
            shrink_end_child = false,
            shrink_start_child = false
        };

        var main_window = new Gtk.ApplicationWindow (this) {
            child = paned,
            default_height = 300,
            default_width = 300,
            titlebar = new Gtk.Grid () { visible = false },
            title = "Hello World"
        };
        main_window.present ();
    }

    public static int main (string[] args) {
        return new MyApp ().run (args);
    }
}
```
{% endcode %}
