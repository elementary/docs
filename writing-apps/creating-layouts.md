---
description: How to organize widgets using common containers
---

# Boxes and Grids

Now that you know how to code, build, and package an app using Vala, Gtk, Meson, and Flatpak, it’s time to learn a little bit more about how to build out your app into something really useful. The first thing we need to learn is how to lay out widgets in our window. But we have a fundamental problem: We can only add one widget (one “child”) to `Gtk.Window`. So how do we get around that to create complex layouts in a Window? We have to add a widget that can contain multiple children. The most common widgets for creating layouts are Boxes and Grids.

## Gtk.Box

A Box is a widget that can contain multiple children in a single row or column. We create a new Gtk.Box like this:

```vala
var box = new Gtk.Box (Gtk.Orientation.VERTICAL, 6);
```

Remember that `Gtk.Button` and `Gtk.Label` accept an argument (a String) in the creation method (that’s the stuff in parentheses and quotes). As shown above, `Gtk.Box` accepts two arguments in the creation method—[orientation](https://valadoc.org/gtk4/Gtk.Orientation) and spacing. Here, we’ve declared that when we add widgets to our box, they should stack vertically.

Let’s add some stuff to the Box:

```vala
var box = new Gtk.Box (Gtk.Orientation.VERTICAL, 6);
box.append (new Gtk.Label ("Label 1"));
box.append (new Gtk.Label ("Label 2"));
```

Add the box to a window using the `child` property:

```vala
var box = new Gtk.Box (Gtk.Orientation.VERTICAL, 6);
box.append (new Gtk.Label ("Label 1"));
box.append (new Gtk.Label ("Label 2"));

var main_window = new Gtk.ApplicationWindow (this) {
    child = box
};
```

Now build your app and see what it looks like. Since we’ve given our box a `Gtk.Orientation` of `VERTICAL` the labels stack up on top of each other. Try creating a `Gtk.Box` with horizontal orientation.

## Gtk.Grid

While we can use `Gtk.Box` to create single row or single column layouts with the append method, we can't use it to create row-and-column-based layouts. Instead, we can use `Gtk.Grid:`

```vala
// First we create all the widgets we want to lay out in our grid
var hello_button = new Gtk.Button.with_label ("Hello");
var hello_label = new Gtk.Label ("Label 1");

var goodbye_button = new Gtk.Button.with_label ("Goodbye");
var goodbye_label = new Gtk.Label ("Label 2");

// Then we create a new Grid and set its spacing properties
var grid = new Gtk.Grid () {
    column_spacing = 6,
    row_spacing = 6
};

// Finally, we attach those widgets to the Grid. Attach first row of widgets
grid.attach (hello_button, 0, 0, 1, 1);
grid.attach (hello_label, 1, 0, 1, 1);

// Attach second row of widgets
grid.attach (goodbye_button, 0, 1);
grid.attach_next_to (goodbye_label, goodbye_button, Gtk.PositionType.RIGHT, 1, 1);

var main_window = new Gtk.ApplicationWindow (this) {
    child = grid
};
```

Notice that the attach method takes 5 arguments:

1. The widget that you want to attach to the grid.
2. The column number to attach to starting at 0.
3. The row number to attach to starting at 0.
4. The number of columns the widget should span.
5. The number of rows the widget should span.

You can also use `attach_next_to` to place a widget next to another one on [all four sides](https://valadoc.org/gtk4/Gtk.PositionType). Note also that providing the number of rows and columns the widget should span is optional. If you supply only the column and row numbers, Gtk will assume that the widget will span 1 column and 1 row.

{% hint style="info" %}
Make sure to give widgets unique names that you’ll remember. It’s best practice to use descriptive names so that people who are unfamiliar with your code can understand what a widget is for without having to know your app inside and out.
{% endhint %}

## Review

Let’s recap what we learned in this section:

* We packed multiple children into a Window using `Gtk.Box` and `Gtk.Grid`
* We set the properties of `Gtk.Grid` including its spacing
* We added multiple widgets into a single Gtk.Grid using the attach method to create complex layouts containing Buttons and Labels.

Now that you understand more about Boxes and Grids, try packing other kinds of widgets into a window like an Image. Don’t forget to play around with the attach method and widgets that span across multiple rows and columns. Remember that Valadoc is super helpful for learning more about the methods and properties associated with widgets.
