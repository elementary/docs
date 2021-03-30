# Creating Layouts

Now that you know how to code, build, and package an app using Vala, Gtk, Meson, and Flatpak, it’s time to learn a little bit more about how to build out your app into something really useful. The first thing we need to learn is how to lay out widgets in our window. But we have a fundamental problem: We can only add one widget \(one “child”\) to `Gtk.Window`. So how do we get around that to create complex layouts in a Window? We have to add a widget that can contain multiple children. One of those widgets is `Gtk.Grid`.

## Widgets Subclass Other Widgets

Before we get into `Gtk.Grid`, let’s stop for a second and take some time to understand Gtk a little better. At the lower level, Gtk has classes that define some pretty abstract traits of widgets such as [`Gtk.Container`](https://valadoc.org/gtk+-3.0/Gtk.Container) and [`Gtk.Orientable`](https://valadoc.org/gtk+-3.0/Gtk.Orientable). These aren’t widgets that we’re going to use directly in our code, but they’re used as building blocks to create the widgets that we do use. It’s important that we understand this, because it means that when we understand how to add children to a `Gtk.Container` like `Gtk.Grid`, we also understand how to add children to a `Gtk.Container` like `Gtk.Toolbar`. Both Grid and Toolbar are widgets that are subclasses of the more abstract class `Gtk.Container`.

If you want to understand more about these widgets and the parts of Gtk that they subclass, jump over to [Valadoc](https://valadoc.org/) and search for a widget like `Gtk.Grid`. See that big tree at the top of the page? It shows you every component of Gtk that `Gtk.Grid` subclasses and even what those components subclass. Having a lower level knowledge of Gtk will help you to implement widgets you haven’t worked with before since you will understand how their parent classes work.

## Gtk.Grid

Now that we’ve gotten that out of the way, let’s get back to our Window and `Gtk.Grid`. Since you’re a master developer now, you can probably set up a new project complete with Meson, push it to GitHub, and create a Flatpak manifest in your sleep. If you want the practice, go ahead and do all of that again. Otherwise, it’s probably convenient for our testing purposes to just play around locally and build from Terminal. So code up a nice `Gtk.Window` without anything in it and make sure that builds. Ready? Let’s add a Grid.

Just like when we add a Button or Label, we need to create our `Gtk.Grid`. As always, don’t copy and paste! Practice makes perfect. We create a new Gtk.Grid like this:

```csharp
var grid = new Gtk.Grid () {
    orientation = Gtk.Orientation.VERTICAL
};
```

Remember that Button and Label accepted an argument \(a String\) in the creation method \(that’s the stuff in parentheses and quotes\). As shown above, `Gtk.Grid` doesn’t accept any arguments in the creation method. However, you can still change the grid’s properties \(like [orientation](https://valadoc.org/gtk+-3.0/Gtk.Orientation)\) as we did on the second line. Here, we’ve declared that when we add widgets to our grid, they should stack vertically.

Let’s add some stuff to the Grid:

```csharp
grid.add (new Gtk.Label (_("Label 1")));
grid.add (new Gtk.Label (_("Label 2")));
```

Super easy stuff, right? We can add the grid to our window using the same method that we just used to add widgets to our grid:

```csharp
main_window.add (grid);
```

Now build your app and see what it looks like. Since we’ve given our grid a `Gtk.Orientation` of `VERTICAL` the labels stack up on top of each other. Try creating a `Gtk.Grid` without giving it an orientation. By default, `Gtk.Grid`’s orientation is horizontal. You really only ever have to give it an orientation if you need it to be vertical.

## Functionality in Gtk.Grid

Okay, so you know all about using a `Gtk.Grid` to pack multiple children into a Window. What about using it to lay out some functionality in our app? Let’s try building an app that shows a message when we click a button. Remember in our first “Hello World” how we changed the label of the button with `button.clicked.connect`? Let’s use that method again, but instead of just changing the label of the button, we’re going to use it to change an empty label to a message.

Let’s create a Window with a vertical Grid that contains a Button and a Label:

```csharp
var button = new Gtk.Button.with_label (_("Click me!"));

var label = new Gtk.Label (null);

var grid = new Gtk.Grid () {
    orientation = Gtk.Orientation.VERTICAL,
    row_spacing = 6
};
grid.add (button);
grid.add (label);

main_window.add (grid);
```

This time when we created our grid, we gave it another property: `row_spacing`. We can also add `column_spacing`, but since we’re stacking widgets vertically we’ll only see the effect of `row_spacing`. Notice how we can create new widgets outside the grid and then pack them into the grid by name. This is really helpful when you start using different methods to change the properties of your widgets.

Now, let’s hook up the button to change that label. To keep our code logically separated, we’re going to add it below `main_window.add (grid);`. In this way, the first portion of our code defines the UI and the next portion defines the functions that we associated with the UI:

```csharp
button.clicked.connect (() => {
    label.label = _("Hello World!");
    button.sensitive = false;
});
```

Remember, we set the button as insensitive here because clicking it again has no effect. Now compile your app and marvel at your newfound skills. Play around with orientation and spacing until you feel comfortable.

## The Attach Method

While we can use `Gtk.Grid` simply to create single row or single column layouts with the add method, we can also use it to create row-and-column-based layouts with the `attach` method. First we’re going to create all the widgets we want to attach to our grid, then we’ll create a new `Gtk.Grid` and set both column and row spacing, and finally we’ll attach our widgets to the grid.

```csharp
var hello_button = new Gtk.Button.with_label (_("Say Hello"));
var hello_label = new Gtk.Label (null);

var rotate_button = new Gtk.Button.with_label (_("Rotate"));
var rotate_label = new Gtk.Label (_("Horizontal"));

var grid = new Gtk.Grid () {
    column_spacing = 6,
    row_spacing = 6
};
```

Make sure to give the Grid, Buttons, and Labels unique names that you’ll remember. It’s best practice to use descriptive names so that people who are unfamiliar with your code can understand what a widget is for without having to know your app inside and out.

```csharp
// add first row of widgets
grid.attach (hello_button, 0, 0, 1, 1);
grid.attach_next_to (hello_label, hello_button, Gtk.PositionType.RIGHT, 1, 1);

// add second row of widgets
grid.attach (rotate_button, 0, 1);
grid.attach_next_to (rotate_label, rotate_button, Gtk.PositionType.RIGHT, 1, 1);

main_window.add (grid);
```

Notice that the attach method takes 5 arguments:

1. The widget that you want to attach to the grid.
2. The column number to attach to starting at 0.
3. The row number to attach to starting at 0.
4. The number of columns the widget should span.
5. The number of rows the widget should span.

You can also use `attach_next_to` to place a widget next to another one on [all four sides](https://valadoc.org/gtk+-3.0/Gtk.PositionType). Note also that providing the number of rows and columns the widget should span is optional. If you supply only the column and row numbers, Gtk will assume that the widget will span 1 column and 1 row.

Don’t forget to add the functionality associated with our buttons:

```csharp
hello_button.clicked.connect (() => {
    hello_label.label = _("Hello World!");
    hello_button.sensitive = false;
});

rotate_button.clicked.connect (() => {
    rotate_label.angle = 90;
    rotate_label.label = _("Vertical");
    rotate_button.sensitive = false;
});
```

You’ll notice in the example code above that we’ve created a 2 x 2 grid with buttons on the left and labels on the right. The top label goes from blank to “Hello World!” and the button label is rotated 90 degrees. Notice how we gave the buttons labels that directly call out what they do to the other labels.

## Review

Let’s recap what we learned in this section:

* We learned about the building blocks of Gtk and the importance of subclasses
* We packed multiple children into a Window using `Gtk.Grid`
* We set the properties of `Gtk.Grid` including its orientation and spacing
* We added multiple widgets into a single Gtk.Grid using the attach method to create complex layouts containing Buttons and Labels that did cool stuff.

Now that you understand more about Gtk, Grids, and using Buttons to alter the properties of other widgets, try packing other kinds of widgets into a window like a Toolbar and changing other properties of [Labels](https://valadoc.org/gtk+-3.0/Gtk.Label) like `width_chars` and `ellipsize`. Don’t forget to play around with the attach method and widgets that span across multiple rows and columns. Remember that Valadoc is super helpful for learning more about the methods and properties associated with widgets.

