# Notifications

By now you've probably already seen the white notification bubbles that appear on the top right of the screen. Notifications are a simple way to notify a user about the state of your app. For example, they can inform the user that a long process has been completed or a new message has arrived. In this section we are going to show you just how to get them to work in your app. Let's begin by making a new project!

## Making Preparations

1. Create a new folder inside of "~/Projects" called "notifications-app"
2. Create a new folder inside of that folder called "src" and add a file inside of it called `Application.vala`
3. Create a `meson.build` file. If you don't remember how to set up Meson, go back to the [previous section](untitled/the-build-system.md) and review.
4. Remember how to [make a .desktop file](untitled/#the-desktop-file)? Excellent! Make one for this project, but this time, since your app will be displaying notifications, add `X-GNOME-UsesNotifications=true` to the end of the file. This is needed so that users will be able to set notification preferences for your app in the system's notification settings.

When using notifications, it's important that your desktop file has the same name as your application's ID. This is because elementary OS uses desktop files to find extra information about the app who sends the notification such as a default icon, or the name of the app. To keep things simple, we'll be using the same RDNN everywhere.

{% hint style="info" %}
If you don't have a desktop file whose name matches the application id, your notification might not be displayed. 
{% endhint %}

## Yet Another Application

In order to display notifications, you're going to need another `Gtk.Application` with a `Gtk.ApplicationWindow`. Remember what we learned in the last few sections and set up a new `Gtk.Application`!

Now that we have a simple window, let's use what we learned in [creating layouts](creating-layouts.md) and make a grid containing one button that will show a notification.

In between `var main_window...` and `main_window.show ();`, write the folowing lines of code:

```csharp
var title_label = new Gtk.Label (_("Notifications"));
var show_button = new Gtk.Button.with_label (_("Show"));

var grid = new Gtk.Grid ();
grid.orientation = Gtk.Orientation.VERTICAL;
grid.row_spacing = 6;
grid.add (title_label);
grid.add (show_button);

main_window.add (grid);
main_window.show_all ();
```

Since we're adding translatable strings, don't forget to update your translation template by running `ninja com.github.yourusername.yourrepositoryname-pot`.

## Sending Notifications

Now that we have a Gtk.Application we can send notifications. Let's connect a function to the button we created and use it to send a notification:

```csharp
show_button.clicked.connect (() => {
    var notification = new Notification (_("Hello World"));
    notification.set_body (_("This is my first notification!"));
    
    send_notification ("com.github.yourusername.yourrepositoryname", notification);
});
```

Okay, now compile your new app. if everything works, you should see your new app. Click the "Send" button. Did you see the notification? Great! Don't forget to commit and push your project in order to save your branch for later.

## Additional Features

Now that you know how to send basic notifications, let's talk about a couple of ways to make your notifications better. Notifications are most useful when users can identify where they came from and they contain relevant information. In order to make sure your notifications are useful, there are three important features you should know about: setting an icon, replacing a notification, and setting priority.

### Icons

In order to make sure users can easily recognize a notification, we should set a relevant icon. Right after the `var notification = New Notification` line, add:

```csharp
var icon = new GLib.ThemedIcon ("dialog-warning");
notification.set_icon (icon);
```

That's it. Compile your app again, and press the "Send" button. As you can see, the notification now has an icon. Using this method, you can set the icon to anything you'd like. You can use `gtk3-icon-browser` to see what system icons are available.

### Replace

We now know how to send a notification, but what if you need to update it with new information? Thanks to the notification ID, we can easily replace a notification. The notification ID should be the same as the app ID that we set in `Gtk.Application`.

Let's make the replace button. This button will replace the current notification with one with different information. Let's create a new button for it, and add it to the grid:

```csharp
var replace_button = new Gtk.Button.with_label (_("Replace"));

grid.add (replace_button);

replace_button.clicked.connect (() => {
    var icon = new GLib.ThemedIcon ("dialog-warning");

    var notification = new Notification (_("Hello Again"));
    notification.set_body (_("This is my second Notification!"));
    notification.set_icon (icon);

    send_notification ("com.github.yourusername.yourrepositoryname", notification);
});
```

Very easy right? Let's compile and run your app again. Click on the buttons, first on "Show", then "Replace". See how the text on your notification changes without making a new one appear?

### Priority

Notifications also have priority. When a notification is set as `URGENT` it will stay on the screen until either the user interacts with it, or you withdraw it. To make an urgent notification, add the following line before the `send_notification ()` function

```csharp
notification.set_priority (NotificationPriority.URGENT);
```

`URGENT` notifications should really only be used on the most extreme cases. There are also [other notification priorities](https://valadoc.org/gio-2.0/GLib.NotificationPriority).

## Review

Let's review what all we've learned:

* We built an app that sends and updates notifications.
* We also learned about other notification features like setting an icon and a notification's priority.

As you could see, sending notifications is very easy thanks to `Gtk.Application`. If you need some further reading on notifications, Check out the page about `Glib.Notification` in [Valadoc](https://valadoc.org/gio-2.0/GLib.Notification).

