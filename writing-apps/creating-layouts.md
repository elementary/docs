# Creating Layouts

Now that you know how to code, build, and package an app using Vala, Gtk, Meson, and Flatpak, it’s time to learn a little bit more about how to build out your app into something really useful. The first thing we need to learn is how to lay out widgets in our window. But we have a fundamental problem: We can only add one widget \(one “child”\) to `Gtk.Window`. So how do we get around that to create complex layouts in a Window? We have to add a widget that can contain multiple children. One of those widgets is `Gtk.Box`.

## Gtk.Box

Now that we’ve gotten that out of the way, let’s get back to our Window and `Gtk.Box`. Since you’re a master developer now, you can probably set up a new project complete with Meson, push it to GitHub, and create a Flatpak manifest in your sleep. If you want the practice, go ahead and do all of that again. Otherwise, it’s probably convenient for our testing purposes to just play around locally and build from Terminal. So code up a nice `Gtk.Window` without anything in it and make sure that builds. Ready? Let’s add a Box.

Just like when we add a Button or Label, we need to create our `Gtk.Box`. As always, don’t copy and paste! Practice makes perfect. We create a new Gtk.Box like this:

```vala
var box = new Gtk.Box (Gtk.Orientation.VERTICAL, 6);
```
—
Remember that Button and Label accepted an argument \(a String\) in the creation method \(that’s the stuff in parentheses and quotes\). As shown above, `Gtk.Box` accepts two arguments in the creation method—[orientation](https://valadoc.org/gtk4/Gtk.Orientation) and spacing. Here, we’ve declared that when we add widgets to our box, they should stack vertically.

Let’s add some stuff to the Box:

```vala
box.append (new Gtk.Label (_("Label 1")));
box.append (new Gtk.Label (_("Label 2")));
```

Add the box to our window using the `child` property:

```vala
main_window.child = box;
```

Now build your app and see what it looks like. Since we’ve given our box a `Gtk.Orientation` of `VERTICAL` the labels stack up on top of each other. Try creating a `Gtk.Box` with horizontal orientation.

## Functionality in Gtk.Box

Okay, so you know all about using a `Gtk.Box` to pack multiple children into a Window. What about using it to lay out some functionality in our app? Let’s try building an app that shows a message when we click a button. Remember in our first “Hello World” how we changed the label of the button with `button.clicked.connect`? Let’s use that method again, but instead of just changing the label of the button, we’re going to use it to change an empty label to a message.

Let’s create a Window with a vertical Box that contains a Button and a Label:

```vala
var button = new Gtk.Button.with_label (_("Click me!"));

var label = new Gtk.Label (null);

var box = new Gtk.Box (Gtk.Orientation.VERTICAL, 6);
box.append (button);
box.append (label);

main_window.child = box;
```

Notice how we can create new widgets outside the box and then pack them into the box by name. This is really helpful when you start using different methods to change the properties of your widgets.

Now, let’s hook up the button to change that label. To keep our code logically separated, we’re going to add it below `main_window.child = box;`. In this way, the first portion of our code defines the UI and the next portion defines the functions that we associated with the UI:

```vala
button.clicked.connect (() => {
    label.label = _("Hello World!");
    button.sensitive = false;
});
```

Remember, we set the button as insensitive here because clicking it again has no effect. Now compile your app and marvel at your newfound skills. Play around with orientation and spacing until you feel comfortable.

## Gtk.Grid

While we can use `Gtk.Box` to create single row or single column layouts with the append method, we can't use it to create row-and-column-based layouts. Instead, we can use `Gtk.Grid`. First we’re going to create all the widgets we want to attach to our grid, then we’ll create a new `Gtk.Grid` and set both column and row spacing, and finally we’ll attach our widgets to the grid.

```vala
var hello_button = new Gtk.Button.with_label (_("Say Hello"));
var hello_label = new Gtk.Label (null);

var swap_button = new Gtk.Button.with_label (_("Swap"));
var swap_label = new Gtk.Label (_("Not Swapped"));

var grid = new Gtk.Grid () {
    column_spacing = 6,
    row_spacing = 6
};
```

Make sure to give the Grid, Buttons, and Labels unique names that you’ll remember. It’s best practice to use descriptive names so that people who are unfamiliar with your code can understand what a widget is for without having to know your app inside and out.

```vala
// add first row of widgets
grid.attach (hello_button, 0, 0, 1, 1);
grid.attach_next_to (hello_label, hello_button, Gtk.PositionType.RIGHT, 1, 1);

// add second row of widgets
grid.attach (swap_button, 0, 1);
grid.attach_next_to (swap_label, swap_button, Gtk.PositionType.RIGHT, 1, 1);

main_window.child = grid;
```

Notice that the attach method takes 5 arguments:

1. The widget that you want to attach to the grid.
2. The column number to attach to starting at 0.
3. The row number to attach to starting at 0.
4. The number of columns the widget should span.
5. The number of rows the widget should span.

You can also use `attach_next_to` to place a widget next to another one on [all four sides](https://valadoc.org/gtk4/Gtk.PositionType). Note also that providing the number of rows and columns the widget should span is optional. If you supply only the column and row numbers, Gtk will assume that the widget will span 1 column and 1 row.

Don’t forget to add the functionality associated with our buttons:

```vala
hello_button.clicked.connect (() => {
    hello_label.label = _("Hello World!");
    hello_button.sensitive = false;
});

swap_button.clicked.connect (() => {
    grid.remove (hello_label);
    grid.remove (swap_label);

    grid.attach_next_to (hello_label, swap_button, Gtk.PositionType.RIGHT, 1, 1);
    grid.attach_next_to (swap_label, hello_button, Gtk.PositionType.RIGHT, 1, 1);

    swap_label.label = _("Swapped");
    swap_button.sensitive = false;
});
```

You’ll notice in the example code above that we’ve created a 2 x 2 grid with buttons on the left and labels on the right. When the top button is clicked the top label goes from blank to “Hello World!” and when the bottom button clicked the labels are swapped. Notice how we gave the buttons labels that directly call out what they do to the other labels.

## Review

Let’s recap what we learned in this section:

* We packed multiple children into a Window using `Gtk.Box` and `Gtk.Grid`
* We set the properties of `Gtk.Grid` including its spacing
* We added multiple widgets into a single Gtk.Grid using the attach method to create complex layouts containing Buttons and Labels that did cool stuff.

Now that you understand more about Gtk, Boxes, Grids, and using Buttons to alter the properties of other widgets, try packing other kinds of widgets into a window like a Image and changing other properties of [Labels](https://valadoc.org/gtk4/Gtk.Label) like `width_chars` and `ellipsize`. Don’t forget to play around with the attach method and widgets that span across multiple rows and columns. Remember that Valadoc is super helpful for learning more about the methods and properties associated with widgets.
