---
description: Creating tool items, GLib.Actions, and keyboard shortcuts
---

# Actions

GTK and GLib have a powerful API called [`GLib.Action`](https://valadoc.org/gio-2.0/GLib.Action.html) which can be used to define the primary actions of your app, assign them keyboard shortcuts, use them as entry points for your app and tie them to [`Actionable`](https://valadoc.org/gtk+-3.0/Gtk.Actionable.html) widgets like Buttons and Menu Items. In this section, we're going to create a Quit action for your app with an assigned keyboard shortcut and a Button that shows that shortcut in a tooltip.

## Gtk.HeaderBar

Begin by creating a `Gtk.Application` with a `Gtk.ApplicationWindow` as you've done in [previous examples](../writing-apps/our-first-app/). Once you have that set up, let's create a new [`Gtk.HeaderBar`](https://valadoc.org/gtk+-3.0/Gtk.HeaderBar.html). Typically your app will have a HeaderBar, at the top of the window, which will contain tool items that users will interact with to trigger your app's actions.

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "Actions",
        titlebar = headerbar
    };
    main_window.present ();
}
```
{% endcode %}

Since we're using this HeaderBar as our app's main titlebar, we need to set `show_title_buttons` to `true` so that GTK knows to include window controls. We can then override our Window's built-in titlebar with the `titlebar` property.

Now, create a new [`Gtk.Button`](https://valadoc.org/gtk+-3.0/Gtk.Button.html) with a big colorful icon and add it to the HeaderBar:

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var button = new Gtk.Button.from_icon_name ("process-stop");
    button.add_css_class (Granite.STYLE_CLASS_LARGE_ICONS);

    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };
    headerbar.pack_start (button);
    
    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "Actions",
        titlebar = headerbar
    };
    main_window.present ();
}
```
{% endcode %}

Build and run your app. You can see that it now has a custom HeaderBar with a big red icon in it. But when you click on it, nothing happens.

{% hint style="info" %}
elementary OS ships with a large set of system icons that you can use in your app for actions, status, and more. You can browse the full set of named icons using the [Icon Browser](https://appcenter.elementary.io/io.elementary.iconbrowser/) app, available in AppCenter.
{% endhint %}

## GLib.SimpleAction

Define a new Quit action and register it with `Application` from inside the `startup` method:

{% code title="Application.vala" %}
```vala
protected override void startup () {
    base.startup ();

    var quit_action = new SimpleAction ("quit", null);

    add_action (quit_action);
    set_accels_for_action ("app.quit",  {"<Control>q", "<Control>w"});
    quit_action.activate.connect (quit);
}
```
{% endcode %}

You'll notice that we do a few things here:

* Instantiate a new [`GLib.SimpleAction`](https://valadoc.org/gio-2.0/GLib.SimpleAction.html) with the name "quit"
* Add the action to our `Gtk.Application`'s [`ActionMap`](https://valadoc.org/gio-2.0/GLib.ActionMap.html)
* Set the "accelerators" (keyboard shortcuts) for "app.quit" to `<Control>q` and `<Control>w`. Notice that the action name is prefixed with `app`; this refers to the namespace of the `ActionMap` built in to `Gtk.Application`
* Connect the `activate` signal of our `SimpleAction` to Application's [`quit ()`](https://valadoc.org/gio-2.0/GLib.Application.quit.html) function.

Now we can tie the action to the HeaderBar Button by assigning the `action_name` property of our Button:

```vala
    var button = new Gtk.Button.from_icon_name ("process-stop") {
        action_name = "app.quit"
    };
```

Build and run your app again and see that you can now quit the app either through the defined keyboard shortcuts or by clicking the Button in the HeaderBar.

{% hint style="info" %}
Accelerator strings follow a format defined by [`Gtk.accelerator_parse`](https://valadoc.org/gtk+-3.0/Gtk.accelerator\_parse.html). You can find a list of key values [on Valadoc](https://valadoc.org/gdk-3.0/Gdk.Key.html)
{% endhint %}

{% hint style="info" %}
Actions defined like this, and registered with Application, can be used as entry points into the app. You can find out more about this integration in [the launchers section](launchers/#actions).
{% endhint %}

## Granite.markup\_accel\_tooltip

You may have noticed that in elementary apps you can hover your pointer over tool items to see a description of the button and any available keyboard shortcuts associated with it. We can add the same thing to our Button with [`Granite.markup_accel_tooltip ()`](https://valadoc.org/granite/Granite.markup\_accel\_tooltip.html).

First, make sure you've included Granite in the build dependencies declared in your `meson.build` file, then set the `tooltip_markup` property of your HeaderBar Button:

{% tabs %}
{% tab title="meson.build" %}
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
{% endtab %}

{% tab title="Application.vala" %}
```vala
var button = new Gtk.Button.from_icon_name ("process-stop") {
    action_name = "app.quit",
    tooltip_markup = Granite.markup_accel_tooltip (
        get_accels_for_action ("app.quit"),
        "Quit"
    )
};
```
{% endtab %}
{% endtabs %}

Build and run your app and then hover over the HeaderBar Button to see its description and associated keyboard shortcuts.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/glib-action). You can also learn more from `GLib.Action` [reference documentation](https://valadoc.org/gio-2.0/GLib.Action.html).
{% endhint %}
