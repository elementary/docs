---
description: Creating tool items, GLib.Actions, and keyboard shortcuts
---

# Actions

GTK and GLib have a powerful API called [`GLib.Action`](https://valadoc.org/gio-2.0/GLib.Action.html) which can be used to define the primary actions of your app, assign them keyboard shortcuts, and tie them to [`Actionable`](https://valadoc.org/gtk+-3.0/Gtk.Actionable.html) widgets in your app like Buttons and Menu Items. In this section, we're going to create a Quit action for your app with an assigned keyboard shortcut and a Button that shows that shortcut in a tooltip. Begin by creating a `Gtk.Application` with a `Gtk.ApplicationWindow` as you've done in previous examples.

## Gtk.HeaderBar

Let's begin by creating a new [`Gtk.HeaderBar`](https://valadoc.org/gtk+-3.0/Gtk.HeaderBar.html). Typically your app will have a HeaderBar at the top of the window that will contain tool items, which users will interact with to trigger your app's actions.

```vala
protected override void activate () {
    var headerbar = new Gtk.HeaderBar () {
        show_close_button = true
    };

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "Actions"
    };
    main_window.set_titlebar (headerbar);
    main_window.show_all ();
}
```

Since we're using this HeaderBar as our app's main titlebar, we need to set `show_close_button` to `true` so that GTK knows to include window controls. We can then override our Window's built-in titlebar with the `set_titlebar ()` method.

Now, still in the activate function, let's create a new [`Gtk.Button`](https://valadoc.org/gtk+-3.0/Gtk.Button.html) with a big colorful icon and add it to our headerbar:

```vala
protected override void activate () {
    var button = new Gtk.Button.from_icon_name ("process-stop", Gtk.IconSize.LARGE_TOOLBAR);

    var headerbar = new Gtk.HeaderBar () {
        show_close_button = true
    };
    headerbar.add (button);

    [...]
}
```

elementary OS ships with a large set of system icons that you can use in your app for actions, status, and more. You can easily browse the full set using the app [LookBook](http://appcenter.elementary.io/com.github.danrabbit.lookbook/), available in AppCenter.

If you compile your app, you can see that it now has a custom HeaderBar with a big red icon in it. But when you click on it, nothing happens.

## GLib.SimpleAction

Let's define a new Quit action by adding the following to the beginning of the `activate ()` method

```vala
var quit_action = new SimpleAction ("quit", null);

add_action (quit_action);
set_accels_for_action ("app.quit",  {"<Control>q", "<Control>w"});
```

and this to the end of the `activate ()` method:

```vala
quit_action.activate.connect (() => {
    main_window.destroy ();
});
```

You'll notice that we do a few things here:
  * Instantiate a new [`GLib.SimpleAction`](https://valadoc.org/gio-2.0/GLib.SimpleAction.html) with the name "quit"
  * Add the action to our `Gtk.Application`'s [`ActionMap`](https://valadoc.org/gio-2.0/GLib.ActionMap.html)
  * Set the "accelerators" (keyboard shortcuts) for "app.quit" to `<Control>q` and `<Control>w"`. Notice that the action name is prefixed with `app`; this refers to the `ActionMap` built in to `Gtk.Application`
  * Connect to the `activate` signal of our `SimpleAction` and call `destroy ()` on `main_window`. This must be at the end of `activate ()` because of that reference to `main_window`

and now we can tie the action to the HeaderBar Button by assigning the `action_name` property of our Button:

```vala
var button = new Gtk.Button.from_icon_name ("process-stop", Gtk.IconSize.LARGE_TOOLBAR) {
    action_name = "app.quit"
};
```

Compile your app again and see that you can now quit the app either through the defined keyboard shortcuts or by clicking the Button in the HeaderBar.

{% hint style="info" %}
Accelerator strings follow a format defined by [`Gtk.accelerator_parse`](https://valadoc.org/gtk+-3.0/Gtk.accelerator_parse.html). You can find a list of key values [on Valadoc](https://valadoc.org/gdk-3.0/Gdk.Key.html)
{% endhint %}

## Granite.markup_accel_tooltip

There's one more thing we can do here to help improve your app's usability. You may have noticed that in elementary apps you can hover your pointer over tool items to see a description of the button and any available keyboard shortcuts associated with it. We can add the same thing to our Button with [`Granite.markup_accel_tooltip ()`](https://valadoc.org/granite/Granite.markup_accel_tooltip.html).

First, make sure you've included Granite in the build dependencies declared in your meson.build file:

```
executable(
    meson.project_name(),
    'src/Application.vala',
    dependencies: [
        dependency('granite'),
        dependency('gtk+-3.0')
    ],
    install: true
)
```

Then, set the `tooltip_markup` property of your HeaderBar Button:

```
var button = new Gtk.Button.from_icon_name ("process-stop", Gtk.IconSize.LARGE_TOOLBAR) {
    action_name = "app.quit",
    tooltip_markup = Granite.markup_accel_tooltip (
        get_accels_for_action ("app.quit"),
        "Quit"
    )
};
```

It may now be clear why we needed to declare our action at the beginning of `activate ()`: before we can get a list of the accelerators associated with the action, we have to define those accelerations and add them to the Application.

Compile your app one last time and hover over the HeaderBar Button to see its description and associated keyboard shortcuts.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/glib-action)
{% endhint %}
