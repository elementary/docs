---
description: Create menus and popovers manually or automatically using a menu model
---

# Popovers

{% hint style="info" %}
Before beginning this section, create a new `Gtk.Application` complete with a desktop launcher file, packaging, etc and add a Quit action as covered in the [Actions](../apis/actions.md) section.
{% endhint %}

## Gtk.Popover

Popovers are a very flexible container widget that can contain any other widgets, just like your main window. In this section we'll be opening a popover by clicking on a [`Gtk.MenuButton`](https://valadoc.org/gtk4/Gtk.MenuButton.html).

<figure><img src="../.gitbook/assets/Screenshot from 2023-03-16 13.14.36.png" alt=""><figcaption></figcaption></figure>

Create a new MenuButton and add it to the end of the HeaderBar. Use the `icon-name` property to give it an icon, and add `Granite.STYLE_CLASS_LARGE_ICONS` to make it bigger just like we did in the [Actions](../apis/actions.md#gtk.headerbar) section with our image Button. You can also set the `primary` property to `true` which will make this menu open when pressing the keyboard shorcut `F10`. Finally, add a tooltip to make the keyboard shortcut more discoverable.

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var menu_button = new Gtk.MenuButton () {
        icon_name = "open-menu",
        primary = true,
        tooltip_markup = Granite.markup_accel_tooltip ({"F10"}, "Menu")
    };
    menu_button.add_css_class (Granite.STYLE_CLASS_LARGE_ICONS);

    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };
    headerbar.pack_end (menu_button);

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "MyApp",
        titlebar = headerbar
    };
    main_window.present ();
}
```
{% endcode %}

Now create a new Button with the action app.quit, but instead of giving it a label, set a [`Granite.AccelLabel`](https://valadoc.org/granite-7/Granite.AccelLabel.html) as its child; this will allow us to show the associated keyboard shortcut for the menu item. Finally, add `Granite.STYLE_CLASS_MENUITEM` so that the button shows as a borderless menu item.

Next create a Popover and set the quit Button as its child. Adding `Granite.STYLE_CLASS_MENU` will automatically set up some margins and sizing for us. Finally, set the `popover` property of the MenuButton to point to our Popover.

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var quit_button = new Gtk.Button () {
        action_name = "app.quit",
        child = new Granite.AccelLabel.from_action_name ("Quit", "app.quit")
    };
    quit_button.add_css_class (Granite.STYLE_CLASS_MENUITEM);

    var popover = new Gtk.Popover () {
        child = quit_button
    };
    popover.add_css_class (Granite.STYLE_CLASS_MENU);

    var menu_button = new Gtk.MenuButton () {
        icon_name = "open-menu",
        popover = popover,
        primary = true
    };
    menu_button.add_css_class (Granite.STYLE_CLASS_LARGE_ICONS);

    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };
    headerbar.pack_end (menu_button);

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "MyApp",
        titlebar = headerbar
    };
    main_window.present ();
}
```
{% endcode %}

Build and run your app. Notice that you can open and close the Popover automatically by activating the MenuButton with your pointer or by using the keyboard shortcut `F10`. The Popover is styled correctly as a menu and clicking the quit Button closes your app.

## GLib.Menu

You can also create Popover menus automatically from Actions. In this section we'll be creating a [Gtk.PopoverMenu](https://valadoc.org/gtk4/Gtk.PopoverMenu.html) with a [GLib.Menu](https://valadoc.org/gio-2.0/GLib.Menu.html) and opening it with a secondary click.

<figure><img src="../.gitbook/assets/Screenshot from 2023-03-16 13.17.08.png" alt=""><figcaption></figcaption></figure>

Start by creating a new Menu and add an item "Quit" with the action name "app.quit". Then create a new PopoverMenu with the Menu we just created as its model. We can `position` and `halign` the PopoverMenu so that it appears where we expect in relation to the pointer, and we'll set `has_arrow` to false, since this menu won't be pointing to a button.

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };

    var menu = new Menu ();
    menu.append ("Quit", "app.quit");

    var popover_menu = new Gtk.PopoverMenu.from_model (menu) {
        halign = Gtk.Align.START,
        has_arrow = false,
        position = Gtk.PositionType.BOTTOM
    };

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "MyApp",
        titlebar = headerbar
    };
    main_window.present ();
}
```
{% endcode %}

Now let's add a new [`Gtk.GestureClick`](https://valadoc.org/gtk4/Gtk.GestureClick.html) and set `Gdk.BUTTON_SECONDARY` as the button it responds to. Then at the end of the `activate ()` function, we'll connect to the `release ()` signal of that gesture.

When the GestureClick is released, it tells us the coordinates of where that gesture took place in our app, which we'll store in a [`Gdk.Rectangle`](https://valadoc.org/gtk4/Gdk.Rectangle.html). Then we can set the `pointing_to` property of our popover to that rectangle, and open it with the `popup ()` function.

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };

    var menu = new Menu ();
    menu.append ("Quit", "app.quit");

    var popover_menu = new Gtk.PopoverMenu.from_model (menu) {
        halign = Gtk.Align.START,
        has_arrow = false,
        position = Gtk.PositionType.BOTTOM
    };

    var secondary_click_gesture = new Gtk.GestureClick () {
        button = Gdk.BUTTON_SECONDARY
    };

    var main_window = new Gtk.ApplicationWindow (this) {
        default_height = 300,
        default_width = 300,
        title = "MyApp",
        titlebar = headerbar
    };
    main_window.present ();

    secondary_click_gesture.released.connect ((n_press, x, y) => {
        var rect = Gdk.Rectangle () {
            x = (int) x,
            y = (int) y
        };

        popover_menu.pointing_to = rect;
        popover_menu.popup ();
    });
}
```
{% endcode %}

There's just one more thing to do for our secondary click menu, and that's to add a widget that will be the parent of our PopoverMenu and receive the GestureClick. Add a Box to the main window, connect the GestureClick using `add_controller ()`, and parent the PopoverMenu with `set_parent ()`.

{% code title="Application.vala" %}
```vala
protected override void activate () {
    var headerbar = new Gtk.HeaderBar () {
        show_title_buttons = true
    };

    var menu = new Menu ();
    menu.append ("Quit", "app.quit");

    var popover_menu = new Gtk.PopoverMenu.from_model (menu) {
        halign = Gtk.Align.START,
        has_arrow = false,
        position = Gtk.PositionType.BOTTOM
    };

    var secondary_click_gesture = new Gtk.GestureClick () {
        button = Gdk.BUTTON_SECONDARY
    };

    var box = new Gtk.Box (Gtk.Orientation.HORIZONTAL, 0);
    box.add_controller (secondary_click_gesture);
    popover_menu.set_parent (box);

    var main_window = new Gtk.ApplicationWindow (this) {
        child = box,
        default_height = 300,
        default_width = 300,
        title = "MyApp",
        titlebar = headerbar
    };
    main_window.present ();

    secondary_click_gesture.released.connect ((n_press, x, y) => {
        var rect = Gdk.Rectangle () {
            x = (int) x,
            y = (int) y
        };

        popover_menu.pointing_to = rect;
        popover_menu.popup ();
    });
}
```
{% endcode %}

Build and run your app, then use a secondary click to open the PopoverMenu at your pointer. You'll notice that the keyboard shortcut is shown automatically in the menu, everything is styled correctly without having to manually add any style classes, and clicking the menu item closes your app.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/popovers). You can also learn more from `Gtk.Popover` [reference documentation](https://valadoc.org/gtk4/Gtk.Popover.html).
{% endhint %}
