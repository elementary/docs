# Packaging

While having a build system is great, our app still isn't ready for regular users. We want to make sure our app can be built and installed without having to use Terminal. What we need to do is package our app. To do this, we use Flatpak on elementary OS. This section will teach you how to package your app as a Flatpak, which is required to publish apps in AppCenter. This will allow normal people to install your app and even get updates for it when you publish them.

## Practice Makes Perfect

If you want to get really good really fast, you're going to want to practice. Repetition is the best way to commit something to memory. So let's recreate our entire Hello World app again _from scratch_:

1. Create a new branch folder "hello-packaging"
2. Set up our directory structure including the "src" and "data" folders.
3. Add your Copying, .desktop, .appdata.xml, icons, and source code.
4. Now set up the Meson build system and translations.
5. Test everything!

Did you commit and push to GitHub for each step? Keep up these good habits and let's get to packaging this app!

## Flatpak Manifest

The Flatpak manifest file describes your app's build dependencies and required permissions.

```yml
app-id: com.github.yourusername.yourrepositoryname
runtime: org.gnome.Platform
runtime-version: '3.38'
base: io.elementary.BaseApp
base-version: juno-20.08
sdk: org.gnome.Sdk
command: com.github.yourusername.yourrepositoryname
finish-args:
  - '--share=ipc'
  - '--socket=fallback-x11'
  - '--socket=wayland'
modules:
  - name: yourrepositoryname
    buildsystem: meson
    sources:
      - type: dir
        path: .
```

That wasn't too bad, right? We'll set up more complicated packaging in the future, but this is all that is required to submit your app to AppCenter Dashboard for it to be built, packaged, and distributed—you don't need to actually create a flatpak bundle yourself. If you'd like you can always read [more about Flatpak](https://docs.flatpak.org/en/latest/introduction.html).
