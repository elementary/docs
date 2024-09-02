# Hello World

The first app we’ll create will be a basic and generic “Hello World”. We’ll walk through the steps of creating folders to store our source code, compiling our first app, and pushing the project to a Git branch. Let’s begin.

## Setting Up

Apps on elementary OS are organized into standardized directories contained in your project's "root" folder. Let's create a couple of these to get started:

1. Create your root folder called "gtk-hello"
2. Create a folder inside that one called "src". This folder will contain all of our source code.

Later on, We'll talk about adding other directories like "po" and "data". For now, this is all we need.

## Gtk.Application

Now what you've been waiting for! We're going to create a window that contains a button. When pressed, the button will display the text "Hello World!" To do this, we're going to use a widget toolkit called GTK and the programming language Vala. Before we begin, we highly recommend that you do not copy and paste. Typing each section manually will help you to practice and remember. Let's get started:

Create a new file in Code and save it as "Application.vala" inside your "src" folder

In this file, we're going to create a special class called a `Gtk.Application`. `Gtk.Application` is a class that handles many important aspects of a Gtk app like uniqueness and the ID you need to identify your app to the notifications server. If you want some more details about `Gtk.Application`, [check out Valadoc](https://valadoc.org/gtk4/Gtk.Application). For now, type the following in "Application.vala".

```vala
public class MyApp : Gtk.Application {
    public MyApp () {
        Object (
            application_id: "io.github.yourusername.yourrepositoryname",
            flags: ApplicationFlags.FLAGS_NONE
        );
    }

    protected override void activate () {
        var main_window = new Gtk.ApplicationWindow (this) {
            default_height = 300,
            default_width = 300,
            title = "Hello World"
        };
        main_window.present ();
    }

    public static int main (string[] args) {
        return new MyApp ().run (args);
    }
}
```

You'll notice that most of these property names are pretty straightforward. Inside `MyApp ()` we set a couple of properties for our `Gtk.Application` object, namely our app's ID and [flags](https://valadoc.org/gio-2.0/GLib.ApplicationFlags.html). The naming scheme we used for our app's ID is called [Reverse Domain Name Notation](https://en.wikipedia.org/wiki/Reverse_domain_name_notation) and will ensure that your app has a unique identifier. The first line inside the `activate` method creates a new `Gtk.ApplicationWindow` called `main_window`. The fourth line sets the window title that you see at the top of the window. We also must give our window a default size so that it does not appear too small for the user to interact with it. Then in our `main ()` method we create a new instance of our `Gtk.Application`and run it.

Ready to test it out? Fire up your terminal and make sure you're in "~/Projects/gtk-hello/src". Then execute the following commands to compile and run your first Gtk app:

```bash
valac --pkg gtk4 Application.vala
./Application
```

Do you see a new, empty window called "Hello World"? If so, congratulations! If not, read over your source code again and look for errors. Also check the output of your terminal. Usually there is helpful output that will help you track down your mistake.

Now that we've defined a nice window, let's put a button inside of it. Add the following to your application at the beginning of the `activate ()` function:

```vala
var button_hello = new Gtk.Button.with_label ("Click me!") {
    margin_top = 12,
    margin_bottom = 12,
    margin_start = 12,
    margin_end = 12
};

button_hello.clicked.connect (() => {
    button_hello.label = "Hello World!";
    button_hello.sensitive = false;
});
```

Then add this line right before `main_window.present ()`:

```vala
main_window.child = button_hello;
```

Any ideas about what happened here?

* We've created a new `Gtk.Button` with the label "Click me!"
* Then we add margins to the button so that it doesn't bump up against the sides of the window.
* We've said that if this button is clicked, we want to change the label to say "Hello World!" instead.
* We've also said that we want to make the button insensitive after it's clicked; We do this because clicking the button again has no visible effect
* Finally, we add the button to our `Gtk.ApplicationWindow` and declare that we want to show the window.

Compile and run your application one more time and test it out. Nice job! You've just written your first GTK app!

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/blob/hello-world/Application.vala)
{% endhint %}

## Pushing to GitHub

After we do anything significant, we must remember to push our code. This is especially important in collaborative development where not pushing your code soon enough can lead to unintentional forks and pushing too much code at once can make it hard to track down any bugs introduced by your code.

First we need to create a new repository on GitHub. Visit [the new repository page](https://github.com/new) and create a new repository for your code.

Open Terminal and make sure you're in your project's root directory "~/Projects/gtk-hello", then issue the following commands

```bash
git init
git add src/Application.vala
git commit -m "Create initial structure. Create window with button."
git remote add origin git@github.com:yourusername/yourrepositoryname.git
git push -u origin master
```

With these commands:

* We've told `git` to track revisions in this folder
* That we'd like to track revisions on the file "Application.vala" specifically
* We've committed our first revision and explained what we did in the revision
* Then we've told `git` to push your code to GitHub.

{% hint style="info" %}
Remember to replace `"yourusername"` with your GitHub username and `"yourrepositoryname"` with the name of the new repository you created
{% endhint %}

## Victory!

Let's recap what we've learned to do in this first section:

* We created a new project containing a "src" folder
* We created our main vala file and inside it we created a new `Gtk.Window` and `Gtk.Button`
* We built and ran our app to make sure that everything worked properly
* Finally, we committed our first revision and pushed code to GitHub

Feel free to play around with this example. Make the window a different size, set different margins, make the button say other things. When you're comfortable with what you've learned, go on to the next section.

## A Note About Libraries

Remember how when we compiled our code, we used the `valac` command and the argument `--pkg gtk4`? What we did there was make use of a "library". If you're not familiar with the idea of libraries, a library is a collection of methods that your program can use. So this argument tells `valac` to include the GTK library \(version 4\) when compiling our app.

In our code, we've used the `Gtk` "Namespace" to declare that we want to use methods from GTK \(specifically, `Gtk.Window` and `Gtk.Button.with_label`\). Notice that there is a hierarchy at play. If you want to explore that hierarchy in more detail, you can [check out Valadoc](https://valadoc.org/gtk4/Gtk.Button).
