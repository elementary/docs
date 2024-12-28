# Packaging

While having a build system is great, our app still isn't ready for regular users. We want to make sure our app can be built and installed without having to use Terminal. What we need to do is package our app. To do this, we use Flatpak on elementary OS. This section will teach you how to package your app as a Flatpak, which is required to publish apps in AppCenter. This will allow people to install your app and even get updates for it when you publish them.

## Practice Makes Perfect

If you want to get really good really fast, you're going to want to practice. Repetition is the best way to commit something to memory. So let's recreate our entire Hello World app again _from scratch_:

1. Create a new branch folder "hello-packaging"
2. Set up our directory structure including the "src" and "data" folders.
3. Add your Copying, .desktop, .metainfo.xml, icons, and source code.
4. Now set up the Meson build system and translations.
5. Test everything!

Did you commit and push to GitHub for each step? Keep up these good habits and let's get to packaging this app!

## Flatpak Manifest

The Flatpak manifest file describes your app's build dependencies and required permissions. Create a `io.github.yourusername.yourrepositoryname.yml` file in your project root with the following contents:

```yaml
# This is the same ID that you've used in meson.build and other files
id: io.github.yourusername.yourrepositoryname

# Instead of manually specifying a long list of build and runtime dependencies,
# we can use a convenient pre-made runtime and SDK. For this example, we'll be
# using the runtime and SDK provided by elementary.
runtime: io.elementary.Platform
runtime-version: '8'
sdk: io.elementary.Sdk

# This should match the exec line in your .desktop file and usually is the same
# as your app ID
command: io.github.yourusername.yourrepositoryname

# Here we can specify the kinds of permissions our app needs to run. Since we're
# not using hardware like webcams, making sound, or reading external files, we
# only need permission to draw our app on screen using either X11 or Wayland.
finish-args:
  - '--share=ipc'
  - '--socket=fallback-x11'
  - '--socket=wayland'

# This section is where you list all the source code required to build your app.
# If we had external dependencies that weren't included in our SDK, we would list
# them here.
modules:
  - name: yourrepositoryname
    buildsystem: meson
    sources:
      - type: dir
        path: .
```

To run a test build and install your app, we can use `flatpak-builder` with a few arguments:

```bash
flatpak-builder build io.github.yourusername.yourrepositoryname.yml --user --install --force-clean
```

This tells Flatpak Builder to build the manifest we just wrote into a clean `build` folder the same as we did for Meson. Plus, we install the built Flatpak package locally for our user. If all goes well, congrats! You've just built and installed your app as a Flatpak.

That wasn't too bad, right? We'll set up more complicated packaging in the future, but this is all that is required to submit your app to AppCenter Dashboard for it to be built, packaged, and distributed. If you'd like you can always read [more about Flatpak](https://docs.flatpak.org/en/latest/introduction.html).

## Uninstalling the application

Ninja and Flatpak both provide commands to uninstalling the application.  It is recommended to use the provided method in both cases.

To remove our application with Flatpak, we will use Flatpak's `remove` command:

```bash
flatpak remove io.github.yourusername.yourrepositoryname
```

Flatpak will prompt you to remove your application.

```bash
flatpak uninstall io.github.yourusername.yourrepositoryname --delete-data


        ID                                                          Branch           Op
 1.     io.github.yourusername.yourrepositoryname                   master           r
 2.     io.github.yourusername.yourrepositoryname.Locale            master           r

Proceed with these changes to the user installation? [Y/n]:
```

> Note: You can append `-y` to the command to bypass the dialog confirmation prompt

If you'd like you can always read [more about Flatpak remove](https://docs.flatpak.org/en/latest/using-flatpak.html#remove-an-application).
