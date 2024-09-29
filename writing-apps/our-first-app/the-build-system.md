---
description: Compiling Binaries & Installing Data with Meson
---

# The Build System

The next thing we need is a build system. The build system that we're going to be using is called [Meson](https://mesonbuild.com/). We already installed the `meson` program at the beginning of this book when we installed `elementary-sdk`. What we're going to do in this step is create the files that tell Meson how to install your program. This includes all the rules for building your source code as well as correctly installing your icons, .desktop, and metainfo files and the binary app that results from the build process.

Create a new file in your project's root folder called "meson.build". We've included some comments along the way to explain what each section does. You don't have to copy those, but type the rest into that file:

```coffeescript
# project name and programming language
project('io.github.yourusername.yourrepositoryname', 'vala', 'c')

# Create a new executable, list the files we want to compile, list the dependencies we need, and install
executable(
    meson.project_name(),
    'src' / 'Application.vala',
    dependencies: [
        dependency('gtk4')
    ],
    install: true
)

# Install our .desktop file so the Applications Menu will see it
install_data(
    'data' / 'hello-again.desktop',
    install_dir: get_option('datadir') / 'applications',
    rename: meson.project_name() + '.desktop'
)

# Install our .metainfo.xml file so AppCenter will see it
install_data(
    'data' / 'hello-again.metainfo.xml',
    install_dir: get_option('datadir') / 'metainfo',
    rename: meson.project_name() + '.metainfo.xml'
)
```

Notice that in each of our `install_data` methods, we rename our files using our project name. By using our project name—and its RDNN scheme—we will ensure that our files are installed under a unique name that won't cause conflicts with other apps.

And you're done! Your app now has a real build system. This is a major milestone in your app's development!

{% hint style="info" %}
Don't forget to add these files to `git` and push to GitHub.
{% endhint %}

## Building and Installing with Meson

Now that we have a build system, let's try it out. Configure the build directory using the Meson command in Terminal:

```bash
meson build --prefix=/usr
```

This command tells Meson to get ready to build our app using the prefix "/usr" and that we want to build our app in a clean directory called "build". The `meson` command defaults to installing our app locally, but we want to install our app for all users on the computer.

Change into the build directory and use `ninja` to build. Then, if the build is successful, install with `ninja install`:

```bash
cd build
ninja
ninja install
```

If all went well, you should now be able to open your app from the Applications Menu and pin it to the Dock. We'll revisit Meson again later to add some more complicated behavior, but for now this is all you need to know to give your app a proper build system. If you want to explore Meson a little more on your own, you can always check out [Meson's documentation](https://mesonbuild.com/Manual.html).

{% hint style="warning" %}
If you were about to add the "build" folder to your git repository and push it, stop! This binary was built for your computer and we don't want to redistribute it. In fact, we built your app in a separate folder like this so that we can easily delete or ignore the "build" folder and it won't mess up our app's source code.
{% endhint %}

## Uninstalling the application

To uninstall your application, change into the build directory and we will use `ninja` once again to uninstall:

```bash
sudo ninja uninstall
```

If all went well, you should see command output that shows files related to your application were removed.  Again, more details can be found in [Meson's documentation](https://mesonbuild.com/Manual.html).

## Review

Let's review all we've learned to do:

* Create a new Gtk app using `Gtk.Window`, `Gtk.Button`, and `Gtk.Label`
* Keep our projects organized into branches
* License our app under the GPL and declare our app's authors in a standardized manner
* Create a .desktop file using RDNN that tells the computer how to display our app in the Applications Menu and the Dock
* Set up a Meson build system that contains all the rules for building our app and installing it cleanly
* Uninstall our application cleanly

That's a lot! You're well on your way to becoming a bona fide app developer for elementary OS. Give yourself a pat on the back, then take some time to play around with this example. Change the names of files and see if you can still build and install them properly. Ask another developer to clone your repo from GitHub and see if it builds and installs cleanly on their computer. If so, you've just distributed your first app! When you're ready, we'll move onto the next section: Translations.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/meson-build-system)
{% endhint %}
