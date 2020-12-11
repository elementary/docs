# Our First App

In the previous chapter, we created a simple "Hello World!" app to show off our vala and Gtk skills. But what if we wanted to share our new app with a friend? They'd have to know which packages to include with the `valac` command we used to build our app, and after they'd built it they'd have to run it from the build directory like we did. Clearly, we need to do some more stuff to make our app fit for people to use, to make it a _real_ app.

## Hello \(again\) World!

To create our first real app, we're going to need all the old stuff that we used in the last example. But don't just copy and paste! Let's take this time to practice our skills and see if we can recreate the last example from memory. Additionally, now that you have the basics, we're going to get a little more complex and a little more organized:

1. Create a new folder inside "~/Projects" called "hello-again".Then, go into "hello-again" and create our directory structure including the "src" folder.
2. Create "Application.vala" in the "src" folder. This time we're going to prefix our file with a small legal header. More about legal stuff later. For now you can copy [the GPL header from our reference documentation](https://elementary.io/docs/code/reference#gpl-header). Be sure to assign the copyright to yourself at the top of the header and change the author to you at the bottom of the header.
3. Now, let's create a `Gtk.Application`, a `Gtk.ApplicationWindow`, and set the window's default properties. Refer back to the last chapter if you need a refresher.
4. For the sake of time let's just put a `Gtk.Label` instead of a `Gtk.Button`. We don't need to try to make the label do anything when you click it.

   ```csharp
   var label = new Gtk.Label ("Hello Again World!");
   ```

   Don't forget to add it to your window and show the window's contents:

   ```csharp
   main_window.add (label);
   main_window.show_all ();
   ```

5. Build "Application.vala" just to make sure it all works. If something goes wrong here, feel free to refer back to the last chapter and remember to check your terminal output for any hints.
6. Initialize the branch, add your files to the project, and write a commit message using what you learned in the last chapter. Lastly, make sure you've created a new repository for your project on GitHub push your first revision with `git`:

   ```bash
   git remote add origin git@github.com:yourusername/yourrepositoryname.git
   git push -u origin master
   ```

Everything working as expected? Good. Now, let's get our app ready for other people to use.

## The .desktop File

Every app comes with a .desktop file. This file contains all the information needed to display your app in the Applications Menu and in the Dock. Let's go ahead and make one:

1. In your project's root, create a new folder called "data".
2. Create a new file in Code and save it in the "data" folder as "hello-again.desktop".
3. Type the following into your .desktop file. Like before, try to guess what each line does.

   ```text
   [Desktop Entry]
   Name=Hello Again
   GenericName=Hello World App
   Comment=Proves that we can use Vala and Gtk
   Categories=Utility;Education;
   Exec=com.github.yourusername.yourrepositoryname
   Icon=application-default-icon
   Terminal=false
   Type=Application
   Keywords=Hello;World;Example;
   ```

   The first line declares that this file is a "Desktop Entry" file. The next three lines are descriptions of our app: The branded name of our app, a generic name for our app, and a comment that describes our app's function. Next, we categorize our app. Then, we say what command will execute it. Finally, we give our app an icon \(a generic one included in elementary OS\) and let the OS know that this isn't a command line app. For more info about crafting .desktop files, check out [this HIG entry](https://elementary.io/docs/human-interface-guidelines#app-launchers).

4. Finally, let's add this file to `git` and commit a revision:

   ```bash
   git add data/com.github.yourusername.yourrepositoryname.desktop
   git commit -am "Add a .desktop file"
   git push
   ```

## AppData.xml

Every app also comes with an .appdata.xml file. This file contains all the information needed to list your app in AppCenter.

1. In your data folder, create a new file called "hello-again.appdata.xml"
2. Type the following into your .appdata.xml file

   ```markup
   <?xml version="1.0" encoding="UTF-8"?>
   <!-- Copyright 2019 Your Name <you@email.com> -->
   <component type="desktop">
     <id>com.github.yourusername.yourrepositoryname</id>
     <metadata_license>GPL</metadata_license>
     <name>Your App's Name</name>
     <summary>A Catchy Tagline</summary>
     <description>
       <p>A quick summary of your app's main selling points and features. Just a couple sentences per paragraph is best.</p>
     </description>
   </component>
   ```

These are all the mandatory fields for displaying your app in AppCenter. There are plenty of other optional fields that you can read about [here](https://www.freedesktop.org/software/appstream/docs/chap-Metadata.html).

There are also some special custom fields for AppCenter to further brand your listing. Specifically, you can set a background color and a text color for your app's header and banner. You can do so by adding the following keys inside the `component` tag:

```markup
<custom>
  <value key="x-appcenter-color-primary">#603461</value>
  <value key="x-appcenter-color-primary-text">rgb(255, 255, 255)</value>
  <value key="x-appcenter-suggested-price">5</value>
</custom>
```

You can specificy colors here in either Hexidecimal or RGB. The background color will automatically be given a slight gradient in your app's banner.

You can also specify a suggested price in whole USD.

{% hint style="info" %}
Remember that AppCenter is a pay-what-you-want store. A suggested price is not a price floor. Users will still be able to choose any price they like, including 0.
{% endhint %}

## Legal Stuff

Since we're going to be putting our app out into the wild, we should include some information about who wrote it and the legal usage of its source code. For this we need a new file in our project's root folder: COPYING.

The COPYING file contains a copy of the license that your code is released under. For elementary OS apps this is typically the [GNU General Public License](https://www.gnu.org/licenses/quick-guide-gplv3.html) \(GPL\). Remember the header we added to our source code? That header reminds people that your app is licensed and it belongs to you. You can choose other licenses like the MIT license as well, but for this example let's stick to the [GPL](https://www.gnu.org/licenses/gpl-3.0.txt).

## Mark Your Progress

Did you remember to add these files to `git` and commit a revision? Each time we add a new file or make a significant change it's a good idea to commit a new revision and push to GitHub. Keep in mind that this acts as a backup system as well; when we push our work to GitHub, we know it's safe and we can always revert to a known good revision if we mess up later.

Now that we've got all these swanky files laying around, we need a way to tell the computer what to do with them. Ready for the next chapter? Let's do this!

