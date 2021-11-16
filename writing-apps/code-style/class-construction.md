# Class Construction

Vala supports two styles of constructing classes:

- Java/C#-style construction
- GObject-style construction

We prefer the GObject-style construction, because this way, we can separate the argument handling from the rest of the initialization logic.

Consider the following example:

```csharp
public class MyClass : Object {
    public int foo { get; construct; }

    public MyClass (int foo) {
        Object (foo: foo);
    }

    construct {
        if (foo > 0) {
            // Do stuff
        } else {
            // Do other stuff
        }
    }
}
```

Here, `MyClass` has a property `foo` of type `int`, which we have declared as `{ get; construct; }`.

This means that since it is a `public` property, the getter `get` is public. With `construct`, we specify that we can only assign its value as a constructor argument. If we try to set this property later after the class is constructed, it would have no effect.

Next up, we have the constructor itself:

```csharp
public MyClass (int foo) {
    Object (foo: foo);
}
```

It receives a value of `foo` as an argument, which is passed when we initialize an object with `new MyClass(foo)`.

Inside the block, we have a special `Object ()` call, in which we specify a property and its value, `Object (foo: foo)`. This sets the value of the integer property defined earlier to the value received as an argument. It is equivalent to saying `this.foo = foo`.

When using GObject-style construction, the `Object ()` call should be the only statement inside the constructor. Any other logic goes inside the `construct` block, which we see next:

```csharp
construct {
    if (foo) {
        // Do stuff
    } else {
        // Do other stuff
    }
}
```

This is where we write all the code that does not deal with the constructor arguments, but is still something we want to run every time an object is created.

### Why is this important?

To understand this, let us look at another example.

Say we want to display a list of storage devices in a file manager. Each device lives in its own row, which is denoted by the class `Row`.

```csharp
public class Row : Object {
    public string name { get; construct; }
    public Icon icon { get; construct; }

    public Row (string name, Icon icon) {
        Object (
            name: name,
            icon: icon
        );
    }

    public Row.from_device (Device device) {
        Object (
            name: device.name,
            icon: device.icon
        );
    }

    construct {
        var icon = new Gtk.Image.from_gicon (icon, Gtk.IconSize.DND);
        var label = new Gtk.Label (name);
    }
}
```

There are two ways to construct this row:

- By manually passing the device name and icon as arguments
- By passing a `Device` object, from which we can derive the name and icon

We can achieve this by making two constructors:

- a default constructor that takes two arguments: `new Row (name, icon)`
- a separate, named constructor which takes a device object: `new Row.from_device (device)`

Note that in both cases, we handle the arguments in the constructor, while the actual logic of creating UI lives in the common `construct` block.

```csharp
construct {
    var icon = new Gtk.Image.from_gicon (icon, Gtk.IconSize.DND);
    var label = new Gtk.Label (name);
}
```

This way, we can have multiple constructors without having to repeat the initialization logic.

## See also

- [GObject-style Construction section in the Vala Tutorial](https://wiki.gnome.org/Projects/Vala/Tutorial#GObject-Style_Construction)
