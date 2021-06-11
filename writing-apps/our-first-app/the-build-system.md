---
description: Compiling Binaries & Installing Data with Meson
---

# The Build System

The next thing we need is a build system. The build system that we're going to be using is called [Meson](https://mesonbuild.com/). We already installed the `meson` program at the beginning of this book when we installed `elementary-sdk`. What we're going to do in this step is create the files that tell Meson how to install your program. This includes all the rules for building your source code as well as correctly installing your icons, .desktop, and appdata files and the binary app that results from the build process.

Create a new file in your project's root folder called "meson.build". We've included some comments along the way to explain what each section does. You don't have to copy those, but type the rest into that file:

```coffeescript
# project name and programming language
project('com.github.yourusername.yourrepositoryname', 'vala', 'c')

# Create a new executable, list the files we want to compile, list the dependencies we need, and install
executable(
    meson.project_name(),
    'src' / 'Application.vala',
    dependencies: [
        dependency('gtk+-3.0')
    ],
    install: true
)

# Install our .desktop file so the Applications Menu will see it
install_data(
    'data' / 'hello-again.desktop',
    install_dir: get_option('datadir') / 'applications',
    rename: meson.project_name() + '.desktop'
)

# Install our .appdata.xml file so AppCenter will see it
install_data(
    'data' / 'hello-again.appdata.xml',
    install_dir: get_option('datadir') / 'metainfo',
    rename: meson.project_name() + '.appdata.xml'
)

# Install our icons in all the required sizes
icon_sizes = ['16', '32', '48', '64', '128']

foreach i : icon_sizes
    install_data(
        join_paths('data', i + '.svg'),
        install_dir: join_paths(get_option('datadir'), 'icons', 'hicolor', i + 'x' + i, 'apps'),
        rename: meson.project_name() + '.svg'
    )
    install_data(
        join_paths('data', i + '.svg'),
        install_dir: join_paths(get_option('datadir'), 'icons', 'hicolor', i + 'x' + i + '@2', 'apps'),
        rename: meson.project_name() + '.svg'
    )
endforeach
```

Notice that in each of our `install_data` methods, we rename our files using our project name. By using our project name—and its RDNN scheme—we will ensure that our files are installed under a unique name that won't cause conflicts with other apps.

You'll notice the section for installing our app icons is a little more complicated. In this example, we're providing SVG icons in all of the required sizes for AppCenter and, since we're using SVG, we're installing them for both LoDPI and HiDPI. Notice that these icons are named `32.svg`, `64.svg`, etc and are stored in our data directory. If you're providing PNG icons instead, you'll need to tweak this part a bit to handle assets exported for use on HiDPI displays. For more information about creating and hinting icons, check out the [Human Interface Guidelines](https://docs.elementary.io/hig/reference/iconography#size).

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

## Review

Let's review all we've learned to do:

* Create a new Gtk app using `Gtk.Window`, `Gtk.Button`, and `Gtk.Label`
* Keep our projects organized into branches
* License our app under the GPL and declare our app's authors in a standardized manner
* Create a .desktop file using RDNN that tells the computer how to display our app in the Applications Menu and the Dock
* Set up a Meson build system that contains all the rules for building our app and installing it cleanly

That's a lot! You're well on your way to becoming a bona fide app developer for elementary OS. Give yourself a pat on the back, then take some time to play around with this example. Change the names of files and see if you can still build and install them properly. Ask another developer to clone your repo from GitHub and see if it builds and installs cleanly on their computer. If so, you've just distributed your first app! When you're ready, we'll move onto the next section: Translations.

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/meson-build-system)
{% endhint %}

