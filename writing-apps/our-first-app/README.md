# Our First App

In the previous chapter, we created a "Hello World!" app to show off our Vala and Gtk skills. But what if we wanted to share our new app with a friend? They'd have to know which packages to include with the `valac` command we used to build our app, and after they'd built it they'd have to run it from the build directory like we did. Clearly, we need to do some more stuff to make our app fit for people to use, to make it a _real_ app.

## Hello (again) World!

To create our first real app, we're going to need all the old stuff that we used in the last example. But don't just copy and paste! Let's take this time to practice our skills and see if we can recreate the last example from memory. Additionally, now that you have the basics, we're going to get a little more complex and a little more organized:

1. Create a new folder inside "\~/Projects" called "hello-again".Then, go into "hello-again" and create our directory structure including the "src" folder.
2.  Create "Application.vala" in the "src" folder. This time we're going to prefix our file with a small legal header. Make sure this header matches the license of your code and assigns copyright to you. More info about this later.

    ```vala
    /*
     * SPDX-License-Identifier: GPL-3.0-or-later
     * SPDX-FileCopyrightText: 2023 Your Name <you@email.com>
     */
    ```
3. Now, let's create a `Gtk.Application`, a `Gtk.ApplicationWindow`, and set the window's default properties. Refer back to the last chapter if you need a refresher.
4.  For the sake of time let's just put a `Gtk.Label` instead of a `Gtk.Button`. We don't need to try to make the label do anything when you click it.

    ```vala
    var label = new Gtk.Label ("Hello Again World!");
    ```

    Don't forget to add it to your window and show the it:

    ```vala
    main_window.child = label;
    main_window.present ();
    ```
5. Build "Application.vala" just to make sure it all works. If something goes wrong here, feel free to refer back to the last chapter and remember to check your terminal output for any hints.
6.  Initialize the branch, add your files to the project, and write a commit message using what you learned in the last chapter. Lastly, make sure you've created a new repository for your project on GitHub push your first revision with `git`:

    ```bash
    git remote add origin git@github.com:yourusername/yourrepositoryname.git
    git push -u origin master
    ```

Everything working as expected? Good. Now, let's get our app ready for other people to use.

## License

Since we're going to be putting our app out into the wild, we should include some information about who wrote it and the legal usage of its source code. For this we need a new file in our project's root folder: the `LICENSE` file. This file contains a copy of the license that your code is released under. For elementary OS apps this is typically the [GNU General Public License](https://www.gnu.org/licenses/quick-guide-gplv3.html) (GPL). Remember the header we added to our source code? That header reminds people that your app is licensed and it belongs to you. GitHub has a built-in way to add several popular licenses to your repository. Read their [documentation for adding software licenses](https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/adding-a-license-to-a-repository) and add a `LICENSE` file to your repository.

{% hint style="info" %}
If you'd like to better understand software licensing, the Linux Foundation offers a [free online course on open source licensing](https://training.linuxfoundation.org/training/open-source-licensing-basics-for-software-developers/)
{% endhint %}

Next, we need a way to create a listing in AppCenter and a way to tell the interface to show our app in the Applications menu and dock: let's write some Metadata.
