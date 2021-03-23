# Human Interface Guidelines {#human-interface-guidelines}

These guidelines are designed to help developers and designers create a beautifully consistent experience on the elementary OS desktop. They were written for interface designers, graphic artists and software developers who will be working on elementary OS. They will not only define specific design elements and principles, but will also instill a philosophy that will help you decide when it is appropriate to deviate from the Guidelines. Adhering to the suggestions contained here will provide many benefits:

* Users will learn to use your application faster, because it shares common elements that they are already familiar with.

* Users will accomplish tasks more quickly, because you will have a straight-forward interface design that isn't confusing or difficult.

* Your application will appear native to the desktop, and share the same elegant look as default applications.

* Your application will be easier to document, because an expected behavior does not require explanation.

* The amount of support you will have to provide, including bugs filed, will be lessened (for the reasons above).

To help you achieve these goals, these guidelines will cover basic interface elements, how to use them and put them together effectively, and how to make your application integrate well with the desktop. The most important thing to remember is that following these guidelines will make it easier to design a new application, not harder.

However, keep in mind that this is a guideline, not a rulebook. New, amazing interaction paradigms appear every day and more are waiting to be discovered. This is a living document that can and will be changed.

For sections that have not yet been written, please refer to [The GNOME HIG](https://developer.gnome.org/hig/stable/)

# Design Philosophy {#design-philosophy}

The elementary OS HIG isn't just about a set of concrete rules; it's meant to be flexible and extensible. As such, this very first portion of the guideline is all about the guiding philosophy we employ. For a quick crash course, we like "The User is Drunk":

<iframe width="560" height="315" src="https://www.youtube.com/embed/r2CbbBLVaPk?rel=0" frameborder="0" allowfullscreen></iframe>

## What Design Is Not {#what-design-is-not}

Before we get into all the things that make up elementary OS apps, there is a clarification that needs to be made. We need to understand what exactly design is about, but more importantly we want to smash two major myths:

1. **Design is not something you add on after you've completed a product.** Whether you realize it or not, you are constantly designing anything you build. It is an intrinsic part of creating something. Design is not just what something looks like. It's not just the colors and fonts. Design is how it works. When you decide to add a button that does a thing, that is design. You made a decision to add a button with an icon or a label and where that button went and the size and color of that button. Decisions are designs.

2. **Design is not just, like, your opinion, man.** Design is testable. One design will meet a specific goal better than another design. Consider different types of bicycles. A folding bicycle has a different set of design goals than a mountain bicycle. Things like weight, size, and tire tread are important factors in helping the intended user reach their goals. Because we understand that design is about solving specific problems, we must also understand that we can objectively compare the effectiveness of two designs at solving those problems.

------------------------------------------
1. [Design Is Not Veneer, Aral Balkan](https://ar.al/notes/design-is-not-veneer/)
2. [Design is Not Subjective, Jeff Law](https://web.archive.org/web/20181208131017/http://www.jefflaw.ca/design-is-not-subjective/)

## Concision {#concision}

Always work to make your app instantly understandable. The main function of your app should be immediately apparent. You can do this in a number of ways, but one of the best ways is by sticking to a principal of concision.

### Avoid Feature Bloat {#avoid-feature-bloat}

It's often very tempting to continue adding more and more features into your application. However, it is important to recognize that every new feature has a price. Specifically, every time you add a new feature:

* Your application gets slower, consumes more resources, and takes up more disk space.
* Your application's interface becomes more cluttered and thus harder to use.
* More time is spent implementing this new feature, rather than perfecting an old feature.
* More code can often mean a greater possibility for bugs.
* More features means more work on documentation, translations, etc.

### Think in Modules {#think-in-modules}

Build small, modular apps that communicate well. elementary OS apps avoid feature overlap and make their functions available to other apps either through [Contractor](#contractor) or over [D-Bus](https://www.freedesktop.org/wiki/Software/dbus/ "View D-Bus docs from FreeDesktop.Org"). This both saves you time as a developer (by other apps making their functions available to you), and is a courteous gesture towards other developers (by making your app's functions available to them).

<span id="avoid-configuration"/>

## Accessible Configuration {#accessible-configuration}

Providing settings can be a way to make sure an app is accessible to a wider set of users with special needs, but it can also be an easy way out of making design decisions about an app's behavior. Just like with problems of feature bloat, settings mean more code, more bugs, more testing, more documentation, and more complexity. When considering adding options to your app, try to strike a balance of making your app more accessible without pushing design work onto your users.

### Build for the "Out of The Box" Experience {#build-for-the-out-of-the-box-experience}

Design with sane defaults in mind. elementary OS apps put strong emphasis on the out of the box experience. If your app has to be configured before a user is comfortable using it, they may not take the time to configure it at all and simply use another app instead.

<iframe width="560" height="315" src="https://www.youtube.com/embed/WD-G6ns8oDU" frameborder="0" allowfullscreen></iframe>

### Ask the Operating System {#ask-the-operating-system}

Get as much information automatically as possible. Instead of asking a user for their name or their location, ask the system for this information. This cuts down on the amount of things a user has to do and makes your app look intelligent and integrated.

<span id="do-you-really-need-it"/>

### Is It Really About Accessibility? {#is-it-really-about-a11y}

Ask yourself if the configuration option you are adding is really necessary to make your app more accessible or if it makes sense to have a user make this decision. Don't ever ask users to make uninformed engineering or design decisions.

### When You Absolutely Have To {#when-you-absolutely-have-to}

Keep things contextual. Instead of tucking away preferences in a configuration dialog, think about displaying them in context with the objects they affect (much like how shuffle and repeat options appear near your music library).

If your app needs to be configured before it can be used (like a mail client), present this configuration inside the main window much like a [Welcome Screen](#welcome-screen). Once again, make sure this is really necessary set-up. Adding unnecessary configuration screens stops users from doing what they wanted to do when they opened your app in the first place.

---------------------------
See Also:

1. [Checkboxes That Kill Your Product](https://limi.net/checkboxes) by Alex Limi
2. [Don't Give Your Users Shit Work](https://zachholman.com/posts/shit-work/) by Zach Holman
3. [The Wizard Anti-Pattern](http://stef.thewalter.net/installer-anti-pattern.html) by Stef Walter

## Minimal Documentation {#minimal-documentation}

Most users don't want to read through help docs before they can use your app. Instead, they expect that your app will be intuitive and simple for them to understand without assistance.

[![](https://imgs.xkcd.com/comics/manuals.png "The most ridiculous offender of all is the sudoers man page, which for 15 years has started with a 'quick guide' to EBNF, a system for defining the grammar of a language. 'Don't despair', it says, 'the definitions below are annotated.'")](https://xkcd.com/1343/)

### Use Understandable Copy {#use-understandable-copy}

Avoid technical jargon and assume little-to-no technical knowledge. This lets your app be "self-documenting."

Provide non-technical explanations instead of cryptic error messages. If something goes wrong, a simplified explanation of what happened and how to fix it should be presented.

------------------------------------------

For more information, see [Writing Style](#writing-style).

# User Workflow {#user-workflow}

Visible design is a large part of the user experience, but so is the user's workflow, or how they interact with an app. In this section, we cover a few important steps of a typical workflow:

* **First-Launch Experience**: What the user sees the first time they use your app.

* **Normal Launch**: What happens when opening your app on a day-to-day basis.

* **Closing**: What happens when closing your app.

* **Background Tasks**: How your app manages to do things invisibly in the background.

## First-Launch Experience {#first-launch-experience}

### Required Configuration {#required-configuration}

When a user first launches an app, they should be able to get down to business as quickly as possible. If configuration is not absolutely required for the first use, they should not be required to configure anything. If configuration is required, they should be presented with a clean and simple [welcome screen](#welcome-screen) within the app. Avoid separate configuration dialogs when launching.

### Speed of Launch {#speed-of-launch}

Your app's first launch is the user's first impression of your app; it's a chance to really show off its design and speed. If your app has to configure things in the background before visibly launching, it gives the user the impression that the app is slow or will take a long time to start up. Instead, focus on making the application window appear fast and ready to be used, then do any background tasks behind the scenes. If the background task is blocking (e.g. the user is unable to perform certain tasks until it's complete), show some type of indication that a background process is happening and make the blocked user interface items insensitive (see: [Widget Concepts](#widget-concepts)).

### Welcoming the User {#welcoming-the-user}

If there is no content to show the user, provide actions they can act upon by using a simple [welcome screen](#welcome-screen). Let them open a document, add an account, import a CD, or whatever makes sense in the context of the app.

### Resetting the App {#resetting-the-app}

If a user explicitly "resets" the app (ex. by deleting all songs in a music library or removing all mail accounts in a mail client), it should return to its first-launch state.

## Normal Launch {#normal-launch}

When a user launches an app, they're performing an explicit action and expecting a fast, oftentimes immediate response. You should focus on three key areas for app launching: speed, obviousness of what to do next, and state.

### Speed {#speed}

As has been said before, speed, especially when launching an app, is very important. There should be as little delay as possible in between the time a user decides to launch an app and the instant they can begin using it. If your app requires a splash screen, you're doing it wrong.

### Obviousness {#obviousness}

When a user launches your app, they should know exactly what to do next. This is achieved by following the other interface guidelines (ensuring your app is consistent with other apps) and by offering up explicit actions from the get go. If the app typically displays "items," such as songs or emails, let the user get at those items by displaying them when the app opens. If there are no previously-opened items, you should offer to open or create a new item (such as a document) by using a [welcome screen](#welcome-screen).

### State {#state}

If the user has previously used your app, it's typically best to restore the state of the app when opening it again. This means the app comes up to right where the user left off so they can pick up their work again. For a music player, this means opening up with the view where the user left it and the song paused where the user closed the app. For a document editor, this would mean opening up with the same document scrolled to the same spot with the cursor in the same spot on the page.

## Always Provide an Undo {#always-provide-an-undo}

Sometimes a user will perform an action which could possibly be destructive or traditionally irreversible. Rather than present the user with a warning, apps should let the user undo the action for an appropriate amount of time. Some prime examples of when this behavior is useful are:

* **Closing an app**. Rather than warning the user, automatically save their work and the app's state so they can return exactly where they left off. See [Closing](#closing).

* **Deleting an item**. Instead of asking the user if they are sure, make the item "disappear" from the app, but provide an easy and intuitive way to undo the delete.

* **Sending an email**. Rather than asking the user if they want to send an email, let them undo or edit the message a short time after "sending."

* **Editing a photo**. Instead of asking the user if they want to destructively apply an edit, let them undo the edit and always keep the original backed up.

This behavior should only as a last resort be implemented by providing a buffer time between when the app shows the user what happened and actually performing the action. To keep the experience responsive, the app should always look as if it performed the action as soon as the user initiates it.

This behavior strikes the best balance of keeping out of the user's way while making sure they don't do something unintended. It's important to keep the undo action unobtrusive yet simple and intuitive; a common way of doing so is by using an info bar, though other methods may also be appropriate.

-----
See also: [Never Use a Warning When you Mean Undo](https://alistapart.com/article/neveruseawarning "Read the article on A List Apart") by Aza Raskin

## Always Saved {#always-saved}

Users should feel confident when using elementary OS; they should know that everything they see is saved and up to date.

Apps in elementary OS should operate around an always-saved state. This means that changes the user makes are instantly applied and visible and that making the user manually save things is a legacy or specialized behavior.

For example, a Song Info dialog should update the track information instantly without a user having to press a save button, user preferences should be applied as soon as the user manipulates the relevant widget, and closing an app should mean that reopening it will return to where the user left off.

## Closing {#closing}

When a user closes an app, it's typically because they're done using it for now and they want to get it out of the way.

### Saving State {#saving-state}

Apps should save their current state when closed so they can be reopened right to where the user left off. Typically, closing and reopening an app should be indistinguishable from the legacy concept of minimizing and unminimizing an app; that is, all elements should be saved including open documents, scroll position, undo history, etc.

Because of the strong convention of saved state, elementary OS does not expose or optimize for legacy minimize behavior; e.g. there is no minimize button, and the Multitasking View does not distinguish minimized windows.

### Closing the App Window {#closing-the-app-window}

Apps should never minimize instead of closing, as that puts the app window into a state that is foreign to users of elementary OS. Instead, windows should close or hide and re-open with a [saved state](#saving-state). Any ongoing or [background tasks](#background-tasks) should be completed soon after the window is closed, then the app should quit so as to not use unnecessary resources.

## Background Tasks {#background-tasks}

If it makes sense to continue a process in the background (such as downloading/transferring, playing music, or executing a terminal command) the app back-end should continue with the task and close when the task is finished. If it's not immediately apparent that the process has completed (as with the file download/transfer or terminal command), the app may show a notification informing the user that the process has completed. If it is apparent, as with the music, no notification is necessary.

If an app performs repeat background tasks (such as a mail client fetching mail), the background tasks should be completed by a daemon and not rely on any window being open.

### Re-Opening the App Window {#re-opening-the-app-window}

If the user re-opens an app while a background process is still executing, the app should be exactly where it would be if the window had been open the whole time. For example, the terminal should show any terminal output, the music player should be on the same page it was when closed, and the browser should come back to the page it was on previously. For more details, see the discussion of app state on a [Normal Launch](#normal-launch).

---

See also: [That's It, We're Quitting](https://blog.ubuntu.com/2011/03/07/quit) by Matthew Paul Thomas

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

```
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

```
GenericName=Markdown Editor
```

### Comment {#comment}

The system uses an app's Comment attribute (found in the .desktop file) to succinctly inform a user what can be done with the app. It should be a short sentence or phrase beginning with a verb and containing the primary nouns that your app deals with. For example, the following are appropriate comments:

* Calendar: **Browse and schedule events**
* Mail: **Send and receive mail**
* Files: **Browse and manage your files**

An app's comment should be in [sentence case](#sentence-case), not include terminal punctuation (periods, exclamation points, or question marks), and should be as short as possible while describing the _primary_ use case of the app:

```
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

```
Categories=Graphics;Photography;Viewer;
```

### Keywords {#keywords}

You may also include keywords in your launcher to help users find your app via search. These follow [the "Keywords" key](https://standards.freedesktop.org/desktop-entry-spec/latest/ar01s05.html) in your .desktop file. For example, a web browser might include "Internet" as a keyword even though it's not in the app's name, generic name, or description. As a result, a user searching for "Internet" will find the app. Here are some more examples:

* Files: **Folders;Browser;Explorer;Finder;Manager;**
* Terminal: **Command;Prompt;Cmd;Emulator;**
* System Settings: **Control;Panel;**

Keywords should be single words (in [title case](#title-case)) separated by and terminated with a semicolon:

```
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

![Dock](/images/docs/human-interface-guidelines/dock-integration/dock.svg)

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

![Indicators](/images/docs/human-interface-guidelines/system-indicators/panel.svg)

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

# Container Widgets {#container-widgets}

## Windows {#windows}

Windows form the foundation of your app. They provide a canvas with basic, built-in actions such as "closing" and "resizing". Although users may see windows as being all the same, elementary OS has several distinct window types. It's important to understand the types of windows available to you, window behavior in general, and behavior that is specific to a certain window type. This section will cover the different types of windows available in elementary OS. Although each type of window is a bit different, think of them all as a subclass of a window. Unless otherwise stated, they all behave as an average window.

* **View Windows** are the most literal kind of "Window" available. They show content such as presentations, web pages, documents, etc., and act as a sort of frame around them. Accordingly, applications typically use multiple view windows to display different instances of content. A "tabbed" view can organize applications that produce many windows.
* **App Windows** are the main windows of apps that are not document-based. Applications rarely use more than a single instance of these windows.
* **Dialogs and Alerts** are temporary windows that require a response from a user. Dialogs and Alerts are especially unique types of windows with much stricter guidelines for their use and visual style. They typically belong to App or View windows and are often modal. We'll discuss what modality means in a bit.

### Window Titles {#window-tiles}

When dealing with window titles, consider that their main use is in distinguishing one window from another. A good window title will provide a description that helps a user make a selection. Keep that in mind as you consider the following:

* A view window should display the name of the content being viewed. For example, a web browser's window title should reflect the title of the current web page. When looking for a specific window among multiple instances of an app, simply showing the application's name is not helpful.
* A window's title should not show the vendor name or version number of the app. Adding the version number or vendor name clutters the title and doesn't help to distinguish a window. Additionally, this information is already available on your app's page in AppCenter.
* Dialogs and alerts should not display a window title. They are distinctive enough in their visual style and are often modal.
* If you need to display more than one item in the title, separate the items with an em dash (—) with space on either side. The helps keep the title clean when you need to show more information.
* Don’t display pathnames in window titles—only the current destination. For instance, it is hard to distinguish between two similar paths when they are displayed in full. If you only show the destination, the distinction is clear.

Even if your app uses a headerbar, be sure to set the window's title; it can be shown in the window switcher and elsewhere in the OS.

## Dialogs {#dialogs}

<div class="dialog">
    <div class="guide horizontal">24</div>
    <img alt="Dialog warning icon" src="images/icons/status/48/dialog-warning.svg">
    <div class="content-area">
        <p class="primary">Primary text providing basic information and a suggestion</p>
        <p>Secondary text providing further details. Also includes information that explains any unobvious consequences of actions.</p>
    </div>
    <div class="guide horizontal" id="dialog-guide-action">24</div>
    <div class="action-area">
        <div class="button">Cancel</div>
        <div class="button suggested-action">Suggested Action</div>
    </div>
</div>

### Alert Text {#alert-text}

An alert contains both primary and secondary text.

The primary text contains a brief summary of the situation and offer a suggested action. This text should use the CSS class `primary`. Primary text should be in [sentence case](#sentence-case) and not include [terminating punctuation](#terminating-punctuation), except in the case of questions.

The secondary text provides a more detailed description of the situation and describes any possible side effects of the available actions. It's important to note that a user should only need the primary text to make a decision and should only need to refer to the secondary text for clarification. This text should be placed one text line height beneath the primary text using the default font size and weight. Secondary text should be in [sentence case](#sentence-case) with [terminating punctuation](#terminating-punctuation).

Make both the primary and secondary text selectable. This makes it easy for the user to copy and paste the text to another window, such as an email message.

Please refer to [`Granite.MessageDialog`](https://valadoc.org/granite/Granite.MessageDialog.html). It is preferred as an elementary OS styled dialog that follows elementary OS design conventions.

### Button Order {#button-order}

![Button Order](/images/docs/human-interface-guidelines/dialogs/button-order.png)

* All dialogs should contain an affirmative button that performs the action suggested in the primary text. This button should be aligned to the very end of the dialog.
* For dialogs that are displayed in response to user action (such as "Quit"), provide a "Cancel" button directly before the affirmative button.
* If your dialog has alternative actions, list them before the "Cancel" button.
* If your dialog has incidental actions (actions that do not close the dialog such as "Help"), these should be aligned to the start of the dialog.

### "OK" is not Okay {#ok-is-not-okay}

When presenting a dialog to a user, always use explicit action names like "Save..." or "Shut Down". Consider how "OK" lets users proceed without understanding the action they are authorizing. Not all users will read the question or information presented to them in a dialog. Using specific action names will make it harder for a user to select an unintended action and may even encourage them to read the presented information before making a selection.

### Suggested and Destructive Actions {#suggested-destructive-actions}

If the primary action is not destructive, its button should be given the `.suggested-action` style class, rendering it in a highlighted style by default. It should usually be focused by default so it's quicker for the user to perform the action using the keyboard.

If the primary action is destructive—i.e. it cannot be easily reversed or undone—it should be given the `.destructive-action` style class, rendering it in a red style by default. Destructive actions should not be focused by default to prevent accidental activation.

Multiple suggested and/or destructive actions should not co-exist in the same context; there should only be one of either type in a dialog.

### Preference Dialogs {#preference-dialogs}

Preference dialogs should be made Transient, but not Modal. When a user makes a change in a preference dialog, the change should be immediately visible in the UI. If the dialog is modal, the user may be blocked from seeing (and especially from interacting with) the change. This means they will have to close the dialog, evaluate the change, then possibly re-open the dialog. By making the dialog transient, we keep the dialog on top for easy access, but we also let the user evaluate and possibly revert the change without having to close and re-open the preference dialog.

---

See also:

1. [Why 'Ok' Buttons In Dialog Boxes Work Best On The Right](http://uxmovement.com/buttons/why-ok-buttons-in-dialog-boxes-work-best-on-the-right/) by UX Movement
2. [Why The Ok Button Is No Longer Okay](http://uxmovement.com/buttons/why-the-ok-button-is-no-longer-okay/) by UX Movement
3. [Should I use Yes/No or Ok/Cancel on my message box?](https://ux.stackexchange.com/questions/9946/should-i-use-yes-no-or-ok-cancel-on-my-message-box) on UX Stack Exchange
4. [Where to Place Icons Next to Button Labels](http://uxmovement.com/buttons/where-to-place-icons-next-to-button-labels/) by UX Movement

## Popovers {#popovers}

Popovers are like a contextual dialog. They display transient content directly related to something that was clicked on and close when clicked out of, like menus.

<img src="/images/docs/human-interface-guidelines/popovers/popover.png" alt="Popovers" style="max-width: 420px"/>

A popover should be used when a user wants to perform a quick action without getting out of the main UI. Some examples of where a popover could be used are adding a contact from an email, adding a bookmark in a browser, or displaying downloads or file transfers.

Popovers should not be used when displaying only a simple list of items; instead, use a menu. Likewise, don't use a popover if the user would spend more than a few seconds in it; instead, use a dialog. Remember that popovers are contextual and should directly relate to the UI element from which they spawn.

## Toolbars {#toolbars}

A Toolbar is useful for providing users with quick access to an app's most used features. Besides Buttons, a Toolbar is one of the most frequently used UI elements. It may seem like a simple container, but it is important to remain consistent in its use and organization.

### Ordering Toolbar Items {#ordering-toolbar-items}

![Toolbar](/images/docs/human-interface-guidelines/toolbars/toolbar.png)

Toolbar items should be organized with the most significant objects on the left and the least significant on the right. If you have many toolbar items it may be appropriate to divide them into groups with space in between each group. Keep in mind that when viewed with RTL languages, your toolbar layout will be flipped.
