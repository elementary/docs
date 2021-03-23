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

![Button Order](https://elementary.io/images/docs/human-interface-guidelines/dialogs/button-order.png)

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

<img src="https://elementary.io/images/docs/human-interface-guidelines/popovers/popover.png" alt="Popovers" style="max-width: 420px"/>

A popover should be used when a user wants to perform a quick action without getting out of the main UI. Some examples of where a popover could be used are adding a contact from an email, adding a bookmark in a browser, or displaying downloads or file transfers.

Popovers should not be used when displaying only a simple list of items; instead, use a menu. Likewise, don't use a popover if the user would spend more than a few seconds in it; instead, use a dialog. Remember that popovers are contextual and should directly relate to the UI element from which they spawn.

## Toolbars {#toolbars}

A Toolbar is useful for providing users with quick access to an app's most used features. Besides Buttons, a Toolbar is one of the most frequently used UI elements. It may seem like a simple container, but it is important to remain consistent in its use and organization.

### Ordering Toolbar Items {#ordering-toolbar-items}

![Toolbar](https://elementary.io/images/docs/human-interface-guidelines/toolbars/toolbar.png)

Toolbar items should be organized with the most significant objects on the left and the least significant on the right. If you have many toolbar items it may be appropriate to divide them into groups with space in between each group. Keep in mind that when viewed with RTL languages, your toolbar layout will be flipped.
