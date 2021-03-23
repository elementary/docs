# Desktop Integration {#desktop-integration}

An important advantage that developers have when choosing elementary OS as their platform is the ability to seamlessly integrate their application with the default desktop. There are several ways in which you can make your app a native experience on elementary OS:

* **Creating an [App Launcher](#app-launchers)**. The primary method of discovering and using your app will be through an app launcher found in the Applications Menu or in the dock. This section details how to create these launchers.

* **MIME handling**. If your application can open and save files, place entries for those file types in the application database and the document type (MIME) database. This lets the file manager and other applications automatically launch your application when they encounter files your application can handle.

* **[Contractor](#contractor)**. elementary OS provides Contractor as an easy way for applications to share files with each other. This will make your application more useful and extend its functionality without adding hundreds of lines of code.

* **Using [System Indicators](#system-indicators)**. elementary OS uses indicators in the panel to provide persistent system-wide information. This section discusses not only how to use that area, but when it is or isn't appropriate to use it.

* **Integrating with the [Dock](#dock-integration)**. elementary OS includes a great dock that supports the Unity Launcher API. This lets your application provide notification badges, progress indicators, and more.

* **Using [Notifications](#notifications)**. Apps in elementary OS can send notifications that play sound and display bubbles to alert the user to certain important events.

## App Launchers {#app-launchers}

The primary method of discovering and using your app will be through an app launcher found in the Applications Menu or in the dock. In order to provide these launchers you must install an appropriate .desktop file with your app. This includes giving your launcher an appropriate name, placing it in the correct category, assigning it an icon, etc.

.desktop files follow the freedesktop.org [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/index.html "View the spec on FreeDesktop.Org"). They should be installed in `/usr/share/applications`.

The contents of .desktop files should follow this formula:

_**Name** is a(n) **GenericName** that lets you **Comment**._

```ini
Name=Eddy
GenericName=Package Installer
Comment=Install Debian packages
Categories=System;PackageManager;
Keywords=Package;Apt;Dpkg;Install;
```

<span id="title"/>

### Name {#Name}

You should not include descriptive words in your app's `Name`. For example, an address book app might be called "Dexter," not "Dexter Address Book." A web browser might be called "Midori," but not "Midori Web Browser." Instead, use the `GenericName` attribute of your app's .desktop file for a generic name, and the `Comment` attribute for a longer descriptive phrase.

```
Name=Dexter
```

### GenericName {#genericname}

If your app is easily categorized or described with a generic name, you should use that for the GenericName attribute in your app's .desktop file. If you can say, "My app is a(n) ________," then whatever fits in that blank could be the generic name. For example, Quilter is a markdown editor, so its generic name is "Markdown Editor".

You should not include articles (the, a, an) or the words "program," "app," or "application" in your app's generic name.

The generic name should be in [title case](#title-case) and may be used around the system to better describe or categorize your app:

```ini
GenericName=Markdown Editor
```

### Comment {#comment}

The system uses an app's Comment attribute (found in the .desktop file) to succinctly inform a user what can be done with the app. It should be a short sentence or phrase beginning with a verb and containing the primary nouns that your app deals with. For example, the following are appropriate comments:

* Calendar: **Browse and schedule events**
* Mail: **Send and receive mail**
* Files: **Browse and manage your files**

An app's comment should be in [sentence case](#sentence-case), not include terminal punctuation (periods, exclamation points, or question marks), and should be as short as possible while describing the _primary_ use case of the app:

```ini
Comment=Listen to music
```

### Categories {#categories}

The following categories may be used to aid with searching or browsing for your app. Keep in mind that you can add more than one and you should add all that apply:

* AudioVideo
* Audio
* Video
* Development
* Education
* Game
* Graphics
* Network
* Office
* Science
* Settings
* System
* Utility

For more info, see the FreeDesktop.Org [menu entry](https://specifications.freedesktop.org/menu-spec/latest/apa.html) spec and the [list of additional categories](https://standards.freedesktop.org/menu-spec/latest/apas02.html)

Categories should be separated by and terminated with a semicolon:

```ini
Categories=Graphics;Photography;Viewer;
```

### Keywords {#keywords}

You may also include keywords in your launcher to help users find your app via search. These follow [the "Keywords" key](https://standards.freedesktop.org/desktop-entry-spec/latest/ar01s05.html) in your .desktop file. For example, a web browser might include "Internet" as a keyword even though it's not in the app's name, generic name, or description. As a result, a user searching for "Internet" will find the app. Here are some more examples:

* Files: **Folders;Browser;Explorer;Finder;Manager;**
* Terminal: **Command;Prompt;Cmd;Emulator;**
* System Settings: **Control;Panel;**

Keywords should be single words (in [title case](#title-case)) separated by and terminated with a semicolon:

```ini
Keywords=Foo;Bar;Baz;
```

---

See also: [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/latest/index.html) from FreeDesktop.org

## Contractor {#contractor}

Contractor is a service and a protocol for exposing services easily between apps. It lets apps interact with other apps and services without hardcoded support for them. You simply add [Contractor](https://github.com/elementary/contractor) support, and then any service registered with Contractor is now available for your app to use. Your app can integrate with Contractor in two different ways:

* Register one of its functions as a service that can be used from other apps
* Implement the contractor menu or otherwise access contractor to receive a list of services that can handle the data your app manages

### Displaying Results from Contractor {#displaying-results-from-contractor}

Contractor results are typically presented to users in menu form. Keep the following in mind when presenting Contractor results:

* If the item acted upon is one of many items visible, such as in an icon view, be sure to display results in a way that is directly related to the item such as in a context menu.
* If the item acted upon is the only item currently visible, such as a web page or a contact in Dexter, contractor can be displayed in a toolbar menu.
* Never display the target app's name. The menu string is the intended display name for that app. Doing so can lead to redundant messages such as, "Set image as wallpaper (wallpaper)" and it is always irrelevant to a user's needs.

## Dock Integration {#dock-integration}

Integrate your app with the dock to communicate its status to the user at a glance.

![Dock](https://elementary.io/images/docs/human-interface-guidelines/dock-integration/dock.svg)

### Progressbars {#progressbars}

Make progress bars unambiguous by referring to a single, specific task. For example, use progress bars to indicate the status of lengthy processes like file transfers and encoding. Do not use progress bars to compound the progress of different types of action.

* **Good Example**: Installation progress in AppCenter
* **Bad Example**: Combined progress of downloading an album, burning a CD, and syncing a mobile device in a music app

### Badges {#badges}

A badge shows a count of actionable items managed by your app. Its purpose is to inform the user that there are items that require user attention or action without being obtrusive. This is a passive notification. A badge should not show totals or rarely changing counters. If the badge is not easily dismissed when the user views your app, it is likely that this is not a good use of a badge.

* **Good Example**: Unread messages in a mail app
* **Bad Example**: Total number of photos in a photo library

## System Indicators {#system-indicators}

Indicators are small icons that live on the top panel. They give users a place to glance for quick information about the state of the system. Selecting an icon opens a small contextual menu with related actions available to the user, including a way to get the the full related system settings.

![Indicators](https://elementary.io/images/docs/human-interface-guidelines/system-indicators/panel.svg)

### Does Your App Need an Indicator? {#does-your-app-need-an-indicator}

Indicators are designed for the system; they display information that is relevant to or affects the general usage of the device. Given that users will probably install many third-party apps, we must be careful about the number of indicators we show and how they behave. Keep in mind that only a very small set of applications need or benefit from an indicator.

**Avoid adding an indicator** if:

* **It will only appear while your app's main window is open.** LibUnity already provides a great API for showing application statuses on your app's icon in the dock. Only use an indicator if it will show while your app's main window is closed.

* **You want a persistent/smaller launcher.** Launchers are already stored in the dock in a way that gives the user control over persistence and size. The indicator area should never be used for an app launcher. If you want to add special actions to your launcher, Quicklists should be used, not an indicator.

* **Your app is for IM, IRC, e-mail, news-reading, or music playback.** Instead, integrate the application with the dock, notifications, or the existing sound menu.

* **You want to show the user your app is running.** Users expect that apps will run in the background when it makes sense. To inform the user of events while your app is running in the background, use notifications.

* **It does not show system-wide information.** App-specific information should be exposed using the dock and/or notifications.

## Notifications {#notifications}

Notifications play a sound and are displayed as bubbles just below the system indicators. They briefly appear on screen where they can be selected to open the relevant app or manually dismissed by hitting the X icon. After a short time, they automatically slide away. Missed notifications can be seen in and cleared from the Notification Center indicator.

### Sounds {#notification-sounds}

Notifications play a system sound by default, but app developers are able to set an appropriate app-specific sound for users to be able to more quickly recognize the source of the notification. Be sure to use the notifications API via LibNotify to set the sound so that it respects user settings and does not play a duplicate sound.

### Icons {#notification-icons}

By default, a notification will include the icon of the app that sent it. For certain apps, it might make sense to display a different relevant image along with a notification, like a user avatar if it's a communication app or album artwork if it's a music app.

### User Control {#user-control}

Keep in mind that users are in ultimate control over notifications and whether or not they appear. Being overly aggressive with your notificiations is a quick way to get the user to turn them off entirely or even uninstall your app.

#### Do Not Disturb {#do-not-disturb}

Users can enable Do Not Disturb mode from Notification Center or System Settings. Do Not Disturb blocks all notification bubbles and sounds until it is manually turned back off.

#### Notification Settings {#notification-settings}

Notification bubbles, sounds, and appearance in Notification Center can each be toggled on or off on a per-app basis from the system notifications settings. Instead of including a global toggle for all notifications in your app, direct the user to the System Settings using a settings URL to open the Notifications page directly.

---

See also:
1. [Developer Tips: Backgrounding &amp; System Integration](https://medium.com/elementaryos/developer-tips-backgrounding-system-integration-31c1f7226606) by Cassidy James Blaede
2. [Farewell to the Notification Area](https://blog.ubuntu.com/2010/04/21/notification-area) by Matthew Paul Thomas
3. [Status Icons and GNOME](https://blogs.gnome.org/aday/2017/08/31/status-icons-and-gnome/) by Allan Day
