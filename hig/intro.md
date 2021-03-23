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

---

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

---

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

---

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

---

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
