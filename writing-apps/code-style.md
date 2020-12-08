---
description: Recommendations for clean code bases
---

# Code Style

Internally, elementary uses the following code style guide to ensure that code is consistently formatted both internally and across projects. Consistent and easily-legible code makes it easier for newcomers to learn and contribute. We'd like to recommend that in the spirit of Open Source collaboration, all Vala apps written in the wider ecosystem also follow these guidelines.

## Whitespace

White space comes before opening parentheses or braces:

```csharp
public string get_text () {}
if (a == 5) {
    return 4;
}

for (i = 0; i < maximum; i++) {}
my_function_name ();
var my_instance = new Object ();
```

Whitespace goes between numbers and operators in all math-related code.

```csharp
c = (n * 2) + 4;
```

Lines consisting of closing brackets \(`}` or `)`\) should be followed by an empty line, except when followed by another closing bracket or an `else` statement.

```csharp
if (condition) {
    // ...
} else {
    // ...
}

// other code
```

## Indentation

### Vala

Vala code is indented using 4 spaces for consistency and readability.

In classes, functions, loops and general flow control, the first brace is on the end of the first line \("One True Brace Style"\), followed by the indented code, and a line closing the function with a brace:

```csharp
public int my_function (int a, string b, long c, int d, int e) {
    if (a == 5) {
        b = 3;
        c += 2;
        return d;
    }

    return e;
}
```

On conditionals and loops, always use braces even if there's only one line of code:

```csharp
if (my_var > 2) {
    print ("hello\n");
}
```

Cuddled else and else if:

```csharp
if (a == 4) {
    b = 1;
    print ("Yay");
} else if (a == 3) {
    b = 3;
    print ("Not so good");
}
```

If you are checking the same variable more than twice, use switch/case instead of multiple else/if:

```csharp
switch (week_day) {
   case "Monday":
       message ("Let's work!");
       break;
   case "Tuesday":
   case "Wednesday":
       message ("What about watching a movie?");
       break;
   default:
       message ("You don't have any recommendation.");
       break;
}
```

### Markup

Markup languages like HTML, XML, and YML should use two-space indentation since they are much more verbose and likely to hit line-length issues sooner.

```markup
<component type="desktop">
  <name>Calendar</name>
  <description>
    <p>A slim, lightweight calendar app that syncs and manages multiple calendars in one place, like Google Calendar, Outlook and CalDAV.</p>
  </description>
  <releases>
    <release version="5.0" date="2019-02-28" urgency="medium">
      <description>
        <p>Add a search entry for calendars</p>
      </description>
    </release>
  </releases>
  <screenshots>
    <screenshot type="default">
      <image>https://raw.githubusercontent.com/elementary/calendar/master/data/screenshot.png</image>
    </screenshot>
  </screenshots>
</component>
```

## Classes and Files

A file should only contain one public class.

All files have the same name as the class in them. For example, a file containing the class `AbstractAppGrid` should be called "AbstractAppGrid.vala"

Classes should be named in a descriptive way, and include the name of any parent classes. For example, a class that subclasses `Gtk.ListBoxRow` and displays the names of contacts should be called `ContactRow`.

## Comments

Comments are either on the same line as the code they reference or in a special line.

Comments are indented alongside the code, and obvious comments do more harm than good.

```csharp
/* User chose number five */
if (c == 5) {
    a = 3;
    b = 4;
    d = -1;  // Values larger than 5 are undefined
}
```

Sometimes detailed descriptions in the context of translatable strings are necessary for disambiguation or to help in the creation of accurate translations. For these situations use `/// TRANSLATORS:` comments.

```csharp
/// TRANSLATORS: The first %s is search term, the second is the name of default browser
title = _("Search for %s in %s").printf (query, get_default_browser_name ());
```

## Variable, Class, and Function Names

Variable and function names are all lower case and separated by underscores:

```csharp
var my_variable = 5;

public void my_function_name () {
   do_stuff ();
}
```

Classes and enums are Upper Camel Case \(aka Pascal Case\):

```csharp
public class UserListBox : Gtk.ListBox {
    private enum OperatingSystem {
        ELEMENTARY_OS,
        UBUNTU
    }
}
```

Constants and enum members should be all uppercase and separated by underscores:

```csharp
public const string UBUNTU = "ubuntu";

private enum OperatingSystem {
    ELEMENTARY_OS,
    UBUNTU
}
```

The values of constant strings \(such as when used with GLib.Action\) should be lowercase and separated with dashes:

```csharp
public const string ACTION_GO_BACK = "action-go-back";
```

## Casting

Avoid using `as` keyword when casting as it might give `null` as result, which could be easily forgotten to check.

```csharp
/* OK */
((Gtk.Entry) widget).max_width_chars

/* NOT OK as this approach requires a check for null */
(widget as Gtk.Entry).max_width_chars
```

## Prefer Properties Over Get/Set Methods

In places or operations where you would otherwise use `get` or `set` , you should make use of `=` instead.

For example, instead of using

```csharp
set_can_focus (false);
```

you should use

```csharp
can_focus = false;
```

### Initialize Objects with Properites
This is especially clearer when initializing an object with many properties. Avoid the following

```vala
var label = new Gtk.Label ("Test Label");
label.set_ellipsize (Pango.EllipsizeMode.END);
label.set_valign (Gtk.Align.END);
label.set_width_chars (33);
label.set_xalign (0);
```

and instead do this

```vala
var label = new Gtk.Label ("Test Label") {
    ellipsize = Pango.EllipsizeMode.END,
    valign = Gtk.Align.END,
    width_chars = 33,
    xalign = 0
};
```

### Create Classes with Properties
This goes for creating methods inside of classes as well. Instead of

```csharp
private int _number;

public int get_number () {
    return _number;
}

public void set_number (int value) {
    _number = value;
}
```

you should use

```csharp
public int number { get; set; }
```

or, where you need some extra logic in the getters and setters:

```csharp
private int _number;
public int number {
    get {
        // We can run extra code here before returning the property. For example,
        // we can multiply it by 2
        return _number * 2;
    }
    set {
        // We can run extra code here before/after updating the value. For example,
        // we could check the validity of the new value or trigger some other state
        // changes
        _number = value;
    }
}
```

Prefering properties in classes enables the use of [`GLib.Object.bind_property ()`](https://valadoc.org/gobject-2.0/GLib.Object.bind_property.html) between classes instead of needing to create signals and handle changing properties manually.

## Vala Namespaces

Referring to GLib is not necessary. If you want to print something instead of:

```csharp
GLib.print ("Hello World");
```

You can use

```csharp
print ("Hello World");
```

## String Formatting

Avoid using literals when formatting strings:

```csharp
var string = @"Error parsing config: $(config_path)";
```

Instead, prefer printf style placeholders:

```csharp
var string = "Error parsing config: %s".printf (config_path);
```

Warnings in Vala use printf syntax by default:

```csharp
critical ("Error parsing config: %s", config_path);
```

## GTK events

Gtk widgets are intended to respond to click events that can be described as "press + release" instead of "press". Usually it is better to respond to `toggle` and `release` events instead of `press`.

## Columns Per Line

Ideally, lines should have no more than 80 characters per line, because this is the default terminal size. However, as an exception, more characters could be added, because most people have wide-enough monitors nowadays. The hard limit is 120 characters.

### Splitting Arguments Into Lines

For methods that take multiple arguments, it is not uncommon to have very long line lengths. In this case, treat parenthesis like brackets and split lines at commas like so:

```text
var message_dialog = new Granite.MessageDialog.with_image_from_icon_name (
    "Basic Information and a Suggestion",
    "Further details, including information that explains any unobvious consequences of actions.",
    "phone",
    Gtk.ButtonsType.CANCEL
);
```

## EditorConfig

If you are using elementary Code or your code editor supports [EditorConfig](https://editorconfig.org/), you can use this as a default `.editorconfig` file in your projects:

```text
# EditorConfig <https://EditorConfig.org>
root = true

# elementary defaults
[*]
charset = utf-8
end_of_line = lf
indent_size = tab
indent_style = space
insert_final_newline = true
max_line_length = 80
tab_width = 4

# Markup files
[{*.html,*.xml,*.xml.in,*.yml}]
tab_width = 2
```
