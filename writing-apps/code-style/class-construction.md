# Class Construction

When creating a new class, prefer GObject-style construction. This type of class construction separates handling arguments from the main logic of your class. It also ensures that information passed in during construction is still available later, and makes your class more likely to be compatible with GLib's functions like property binding.

A simple class written with GObject-style construction looks like this:

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

## Construction Properties

```csharp
public int foo { get; construct; }
```

In the above example, `MyClass` has a `public` property `foo` whose type is `int`, but we've also declared it as `{ get; construct; }`. This shorthand declares the type of access for this property's `get` and `set` functions. Since we declared the property as `public`, `get` is also public, but we've declared the `set` function as `construct`, which means we can only assign its value as a constructor argument. 

We want construction arguments to be public to ensure that we can later get information out of a class that was used to construct it, if need be. But it's important to declare limited `set` access on properties for future maintainability. Since there is no handling for changes in this property in the `construct` block, setting this property after the class is constructed would have no effect, even if we allowed it by declaring `{ get; construct set; }`.

## Constructors

```csharp
public MyClass (int foo) {
    Object (foo: foo);
}
```

`MyClass` also contains a constructor; it describes what arguments are required to construct the class. We've declared here that in order to construct `MyClass`, we need an `int` passed in when we initialize a new object such as with `var new_class = new MyClass (5);`


Inside the constructor, we have a special `Object ()` call, in which we specify a property and its value, `Object (foo: foo);`. This sets the value of the integer property `foo` defined earlier to the value received as an argument. It is equivalent to saying `this.foo = foo;`.

## The Construct Block

```csharp
construct {
    if (foo) {
        // Do stuff
    } else {
        // Do other stuff
    }
}
```

When using GObject-style construction, a constructor should only contain code that parses arguments and sets property values with the `Object ()` call. All other class contruction logic happens in the `construct` block. Code in the `construct` block runs every time an instance of this object is created, regardless of the constructor used.

## Declaring Multiple Constructors

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

To see how this style of class construction scales and keeps code organized, let's look at a more complex example. Say we want to display a list of devices. Each device lives in its own row, which is denoted by the class `Row`. In this example, sometimes we have access to a `Device` object which stores the `name` and `icon` properties of the device, but for some devices we have to pass in those properties manually to construct our row. We can acheive this by declaring two constructors:

- a default constructor that takes two arguments: `string name` and `Icon icon`
- a separate, named constructor which takes one argument: a `Device` object

```csharp
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
```

Note that in both cases, we handle the arguments in the constructor, while the actual logic of creating UI widgets lives in the common `construct` block.

```csharp
construct {
    var icon = new Gtk.Image.from_gicon (icon, Gtk.IconSize.DND);
    var label = new Gtk.Label (name);
}
```

This way, we can have multiple constructors without having to repeat the initialization logic.

## See also

- [GObject-style Construction section in the Vala Tutorial](https://wiki.gnome.org/Projects/Vala/Tutorial#GObject-Style_Construction)
