# Packaging

While having a build system is great, our app still isn't ready for regular users. We want to make sure our app can be built and installed without having to use Terminal. What we need to do is package our app. To do this, we use the Debian packaging format \(.deb\) on elementary OS. This section will teach you how to package your app as a .deb file, which is required to publish apps in AppCenter. This will allow normal people to install your app and even get updates for it when you publish them.

## Practice Makes Perfect

If you want to get really good really fast, you're going to want to practice. Repetition is the best way to commit something to memory. So let's recreate our entire Hello World app again _from scratch_:

1. Create a new branch folder "hello-packaging"
2. Set up our directory structure including the "src" and "data" folders.
3. Add your Copying, .desktop, .appdata.xml, and source code.
4. Now set up the Meson build system and translations.
5. Test everything!

Did you commit and push to GitHub for each step? Keep up these good habits and let's get to packaging this app!

## Debian Control

Now it's time to create the rules that will allow your app to be built as a .deb package. Let's dive right in:

1. elementary maintains a simple version of the "debian" folder that contains all the files we need for packaging. Let's grab a copy of that with `git`:

   ```bash
   git clone git@github.com:elementary/debian-template.git
   ```

2. Copy the "debian" folder from that branch into your "hello-packaging" folder.
3. Use the tool `dch -i` to update your changelog. It should automatically generate something like below:

   ```text
   com.github.yourusername.yourrepositoryname (0.1) bionic; urgency=medium

     * Initial Release.

    -- Your Name <you@emailaddress.com>  Monday, 8 Apr 2019 04:53:39 -0500
   ```

   The first line contains your app's binary name, version, OS codename, and how urgently your package should be built. After the `*` is a list of your changes. Finally, you include your name, email address, and the date. For more information about the debian changelog, make sure to read the [documentation](https://www.debian.org/doc/debian-policy/#document-ch-source).

4. Open the file called "control" and make it look like below:

   ```text
   Source: com.github.yourusername.yourrepositoryname
   Section: x11
   Priority: optional
   Maintainer: Your Name <you@emailaddress.com>
   Build-Depends: debhelper (>= 10.5.1),
                  gettext,
                  libgtk-3-dev (>= 3.10),
                  meson,
                  valac (>= 0.28.0)
   Standards-Version: 4.1.1

   Package: com.github.yourusername.yourrepositoryname
   Architecture: any
   Depends: ${misc:Depends}, ${shlibs:Depends}
   Description: Hey young world
    This is a Hello World written in Vala using Meson build system.
   ```

5. Open the file called "copyright". We only need to edit what's up top:

   ```text
   Format: http://dep.debian.net/deps/dep5
   Upstream-Name: hello-packaging
   Source: https://github.com/yourusername/yourrepositoryname

   Files: src/* data/* debian/*
   Copyright: 2019 Your Name <you@emailaddress.com>
   License: GPL-3.0+
   ```

That wasn't too bad, right? We'll set up more complicated packaging in the future, but this is all that is required to submit your app to AppCenter Dashboard for it to be built, packaged, and distributed—you don't need to actually create a .deb file yourself. If you'd like you can always read [more about Debian packaging](https://www.debian.org/doc/debian-policy/).

{% hint style="info" %}
Note that Debian packaging is _very_ picky about whitespace, so if you're running into errors, make sure you're not adding, changing, or removing whitespace from the original template files.
{% endhint %}

If you're packaging your app for elementary OS 0.4 Loki, you will also need to update the "rules" file to work with Meson. You can see an example [here](https://github.com/cassidyjames/principles/blob/ef84ed129bdeaec613b0c457c766cb9aa9ac1bfb/debian/rules). If you are targeting elementary OS 5.0 Juno or newer, this is not necessary.

