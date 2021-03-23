# UI Toolkit Elements

elementary OS uses consistent user interface \(UI\) elements to bring a unified and predictable experience to users, no matter what app they're using. When used properly, this ensures a small \(or nonexistent\) learning curve for your app.

## Widget Concepts <a id="widget-concepts"></a>

Before we get into all the widgets available in elementary OS, we need to cover some basic concepts that apply to all widgets. Employing these concepts correctly will create a more seamless experience for your users and help you avoid sifting through bug reports!

### Controls That Do Nothing <a id="controls-that-do-nothing"></a>

A very common mistake for developers to make is creating controls that seemingly do nothing. Keep in mind that we want to present an environment where users feel comfortable exploring. A curious user will interact with a control expecting there to be some immediate reaction. When a control seemingly does nothing, this creates confusion and can be scary \(Think, "uh-oh I don't know what happened!"\). In some cases, controls that do nothing are simply clutter and add unnecessary complexity to your UI.

Consider the "clear" button present in [search fields](ui-toolkit-elements.md#search-fields). This button only appears when it is relevant and needed. Clicking this button when the field is already clear essentially does nothing.

### Sensitivity <a id="sensitivity"></a>

Sometimes it doesn't make sense for a user to interact with a widget until some pre-requisite is fulfilled. For example, It doesn't make sense to let a user click a browser's "Forward" button unless there is forward history available. In this case, you should make the "Forward" button insensitive or a user may click it, expecting a result, and be confused when nothing happens.

It's usually better to make a widget insensitive than to hide it altogether. Making a widget insensitive informs the user that the functionality is available, but only after a certain condition is met. Hiding the widget gives the impression that the functionality is not available at all or can leave a user wondering why a feature has suddenly "disappeared".

### Hidden Widgets <a id="hidden-widgets"></a>

When a widget only makes sense in a certain context \(not as an indicator of an action to be performed\) sometimes it does make more sense to hide it. Take hardware requirements for example: It may not make sense to show multi-display options if the system only has a single display. Making multi-display options insensitive is not really a helpful hint on this system. Another exemption to this rule is a widget that a user will only look for in context, like the clear button example above. Finally, Keep in mind that insensitive items will still be recognized by screen readers and other assistive tech, while hidden widgets will not.

### Spacing <a id="spacing"></a>

* Windows should have a 12px \(minimum\) space between any widgets and the window's border.
* Labels should be 12px \(minimum\) from their widgets.
* If there are section headers present, labels should be indented.
* Horizontal spacing between buttons is 6px.

### Alignment <a id="alignment"></a>

* Widgets should be "Justified" so that they align on both the left and right sides. Do not include "descriptor" widgets such as icons or labels in this justification.
* Labels should be right aligned with respect to each other when possible.
* Section headers should be left aligned with respect to each other.

See also: [Form Label Proximity: Right Aligned is Easier to Scan](http://uxmovement.com/forms/form-label-proximity-right-aligned-is-easier-to-scan) by UX Movement

## Infobars <a id="infobars"></a>

Infobars provide contextual information and actions to the user with varying levels of severity.

![Infobars](https://elementary.io/images/docs/human-interface-guidelines/infobars/infobars.png)

It is important to determine the severity or type of infobar to use. There are four types of infobars available:

* **Information**: Supplemental information and an optional action the user may perform. Shows as white in the UI.
* **Question**: A non-critical question for the user. An answer of some sort is expected, but it's not urgent or severe. Shows as blue in the UI.
* **Warning**: Lets the user know something unexpected or bad may happen and provides an action to resolve it. Displays as yellow in the UI.
* **Error**: Informs the user of an error that has occurred and requires a user action to resolve it. Reserved for critical situations. Displays as red in the UI.

## Welcome Screen <a id="welcome-screen"></a>

![Welcome Screen](https://elementary.io/images/docs/human-interface-guidelines/welcome-screen/welcome-screen.png)

The Welcome Screen is a friendly way to help users get started with your app.

### Usage <a id="welcome-screen-usage"></a>

Typically a Welcome Screen is used for apps like Music or Code where you have to import or create objects in a library before you can interact with them. This provides your users with a clear path to getting started and points out any immediate steps they must take before your app becomes useful.

If your app lets users clear its library, make sure that it returns to the Welcome Screen instead of an empty list.

### Labeling <a id="welcome-screen-labeling"></a>

The Welcome Screen consists of two sets of labels:

* The first set explains the situation and what the Welcome Screen will help you accomplish. As an example, Music's Welcome Screen explains that your music library is empty and that in order for the library view to become useful, we must import songs into our library.
* The second set of labels consists of the actions that will assist a user in getting started with your app. To use Music as an example again, one possible action is setting your music folder to an alternate location. First we name the action, "Change Music Folder". Then, we describe what the action does, "Load music from a folder, a network or an external disk."

### Iconography <a id="welcome-screen-iconography"></a>

Grouped with each action is an icon that helps to quickly visualize it. Most of the time these will be Action icons, but you can use Places icons when importing or setting a location and even Apps icons if you must open a configuration utility.

## Source List <a id="source-list"></a>

A source list may be used as a high-level form of navigation. Source lists are useful for showing different locations, bookmarks, or categories within your app.

![Source List in Files](https://elementary.io/images/docs/human-interface-guidelines/source-list/files.png)

### Sections <a id="sections"></a>

A source list may be separated into different collapsible sections, each with its own heading. For example, a file manager might have a section for bookmarked locations, a section for storage devices attached to the computer, and a section for network locations. These sections help group related items in the source list and lets the user hide away sections they might not use.

Avoid nesting expandable sections within a source list if possible; if you find yourself wanting to do this, you may need to rethink the sections.

### Hierarchy <a id="hierarchy"></a>

Hierarchy is important with source lists, both within the widget itself and within the broader scope of your app.

Sections in the source list should be sorted from most important at the top to least important at the bottom. If you're having a hard time deciding the relative importance of each section, think about which section a user is likely to use more often. Sorting the sections this way ensures that the most important items are always visible, even if the source list is too short to fit all of the items, though of course items at the bottom will still be accessible via scrolling.

A source list goes at the left side of a window \(or right side for right-to-left languages\). Because the user reads in this direction, the sidebar is reinforced as being before \(and therefore at a higher level than\) the app's contents.

## Buttons <a id="buttons"></a>

Buttons are an incredibly important widget to understand since your app will undoubtedly contain them.

### Tool Buttons <a id="tool-buttons"></a>

![Open button](https://elementary.io/images/docs/human-interface-guidelines/buttons/open.png)

#### Labeling <a id="tool-buttons-labeling"></a>

Tool Buttons are almost always icon-only and do not provide a button border. They should not be accompanied by a label.

#### Tooltips <a id="tool-buttons-tooltips"></a>

All Tool Buttons should have tooltips, since they do not contain a label. This assists users with disabilities as well as giving a translation for an unrecognized icon. Tooltips should be done in [sentence case](ui-toolkit-elements.md#sentence-case) without [terminating punctuation](ui-toolkit-elements.md#terminating-punctuation).

Like text button labels, a tooltip should clearly describe what will happen when the button is pressed.

**Keyboard Shortcuts**

If a button has a related keyboard shortcut that will perform the same action, its tooltip should include the shortcut. See [`Granite.markup_accel_tooltip ()`](https://valadoc.org/granite/Granite.markup_accel_tooltip.html) for specifics.

### Text Buttons <a id="text-buttons"></a>

![Cancel button](https://elementary.io/images/docs/human-interface-guidelines/buttons/cancel.png)

#### Labeling <a id="text-buttons-labeling"></a>

Text Button labels should be done in [title case](ui-toolkit-elements.md#title-case).

Like menu items, Text Button labels should consist of an Action or a Location but not a status. Make sure that a button's label clearly describes what will happen when it is pressed.

"Remove Account", "Transfer to Jim's Laptop", and "Import 20 Songs" are good labels.

"OK", "Get more storage!", and "Low Battery" are not good button labels. The "Get more storage!" label has incorrect capitalization and unnecessary punctuation. The other two labels do not indicate what will happen as a result of clicking the button.

#### Tooltips <a id="text-buttons-tooltips"></a>

Since Text buttons have a clear and explicit label, it's usually unnecessary to give them a tooltip.

1. [Why The OK Button Is No Longer Okay](http://uxmovement.com/buttons/why-the-ok-button-is-no-longer-okay/) by UX Movement
2. [Should I use Yes/No or Ok/Cancel on my message box?](https://ux.stackexchange.com/questions/9946/should-i-use-yes-no-or-ok-cancel-on-my-message-box) on UX Stack Exchange

### Back Buttons <a id="back-buttons"></a>

![Back Button](https://elementary.io/images/docs/human-interface-guidelines/buttons/back-button.png)

A back button is simply a text button with a special style class. It should be used to navigate back to a previous view, typically from a child view to the main view.

The text of the button should be the title of the previous view. If the button will take the user “home,” then “Home” may be an appropriate label; however a more specific label is preferred, such as “All Settings” in System Settings. Avoid just using “Back,” as the back button is visually distinct and going back is already implied by its unique shape. The button should tell the user _where_ it will take them back to.

When using a back button, it is recommneded to use a Gtk.Stack to switch between views. This way you get a sliding animation further representing the backwards progression when activating the back button.

Back buttons are typically seen in headerbars, but can be used in other contexts as well.

## Search Fields <a id="search-fields"></a>

Apps that support the searching or filtering of content should include a search field on the right side of the app's toolbar. This gives users a predictable place to see whether or not an app supports searching, and a consistent location from which to search. Gtk+ provides a convenient complex widget for this purpose called [Gtk.SearchEntry](https://valadoc.org/gtk+-3.0/Gtk.SearchEntry.html).

![Search Field](https://elementary.io/images/docs/human-interface-guidelines/search-fields/search-field.png)

### Distinguish Between Search and Find <a id="distinguish-search-find"></a>

Search is for filtering the contents of a library, i.e. Music or Videos, to the matching items. Search is typically initiated when typing anywhere in a library view.

Find is for highlighting matching instances of a string, i.e. in a text editor, web page, or Terminal. It is triggered by a keyboard shortcut \(Ctrl+F\) or with a search icon. The find bar appears in a revealer below the headerbar with relevant actions such as find next, find previous, highlight all, etc. The revealer may also contain other relevant actions such as replace or go to line.

### Behavior <a id="behavior"></a>

If it is possible to include search functionality within your app, it is best to do so. Any list or representation of multiple pieces of data should be searchable using a search field that follows these rules:

* Results should be instantly shown as you type. This helps your app to appear faster and is more useful than having to hit "Enter" and wait. Exceptions may be made if the data is not stored locally.
* In most cases, the search should be case-insensitive; users should not be expected to provide the exact capitalization. A good compromise is "smart case" where case is respected whenever the user intentionally types lower and upper case letters.

### Labeling <a id="search-fields-labeling"></a>

Search fields should contain hint text that describes what will be search. You can set this using the entry property ["placeholder\_text"](https://valadoc.org/gtk+-3.0/Gtk.Entry.placeholder_text.html).

Most search fields should use the format "Search OBJECTS" where OBJECTS is something to be searched, like Contacts, Accounts, etc.

If the search field interacts with a search service, the hint text should be the name of that service such as "Google" or "Yahoo!"

## Selection Controls <a id="selection-controls"></a>

Selection controls present a way for users to select or enable options. There are several types of selection controls available in elementary OS:

* **Checkboxes** present a way for users to select multiple items from a set.
* **Comboboxes** and **Radio buttons** present a way for users to select a single option from a set.
* **Linked buttons** present a compact way for users to select options that can be described by an icon or with only one or two words.
* **Switches** present a way for users to toggle certain features or behaviors "on" or "off".

### Checkboxes <a id="checkboxes"></a>

![Checkboxes](https://elementary.io/images/docs/human-interface-guidelines/selection-controls/checkboxes.png)

Use checkboxes when users are making a selection of items. If you have a single option, avoid using a checkbox and use a switch instead.

Make sure that users can toggle the state of the checkbox by clicking on the label associated with the checkbox.

Labels associated with checkboxes should usually be nouns or nounal phrases.

### Comboboxes <a id="comboboxes"></a>

![Comboboxes](https://elementary.io/images/docs/human-interface-guidelines/selection-controls/comboboxes.png)

Use a combobox \(also called a dropdown\) when:

* Users are selecting only a single item from a set and
* The set is too long to show all available options at once

### Linked Buttons <a id="linked-buttons"></a>

![Linked Buttons](https://elementary.io/images/docs/human-interface-guidelines/selection-controls/linked_buttons.png)

Use linked buttons when:

* All options can be described by an icon or with only one or two words and
* You think users should see all available options at once.

Linked buttons can be used to select multiple related options like "Bold", "Italic", and "Underline", or they can be used to select a single mutually exclusive option \(also called a mode button\) like Grid, List, or Column view.

Linked buttons should never contain colored icons. Only 16px symbolic icons OR text. Do not mix icons and text.

### Radio Buttons <a id="radio-buttons"></a>

![Radio Buttons](https://elementary.io/images/docs/human-interface-guidelines/selection-controls/radio_buttons.png)

Use radio buttons when:

* Users are selecting only a single item from a set and
* You think users should see all available options at once.

### Switches <a id="switches"></a>

![Switches](https://elementary.io/images/docs/human-interface-guidelines/selection-controls/switches.png)

Use a switch when users are toggling certain features or behaviors "on" or "off".

Don't use switches to select related items as part of a list, instead use a checkbox. Think of switches as acting on independent services and checkboxes as including objects in a list. This is an important distinction to make.

When possible, directly call out the service you are acting on. Do not use words that describe the state that the widget is describing like "Enable Multitouch", "Use Multitouch", or "Disable Multitouch". This can create a confusing situation logically. Instead, simply use the noun and write "Multitouch".

#### Mode Switches <a id="mode-switches"></a>

![Mode Switches](https://elementary.io/images/docs/human-interface-guidelines/selection-controls/mode-switches.png)

As of elementary OS 5 Juno, mode switches are a new switch-based widget that communicate switching between two distinct states. For example, switching between a photo or video mode in a camera. The switch is drawn smaller and inline with the provided symbolic icons. Tooltip hints can also be provided when hovering the icons.

Use a mode switch when you have two distinct and opposing states that you are switching between that can be effectively communicated via an icon. If you need text or more than two states, use a [Mode Button](ui-toolkit-elements.md#linked-buttons) instead.

See also: [_3 Ways to Make Checkboxes, Radio Buttons Easier to Click_](http://uxmovement.com/forms/ways-to-make-checkboxes-radio-buttons-easier-to-click/) by UX Movement

## Notebooks <a id="notebooks"></a>

Notebooks are a type of widget that lets apps show one of multiple pages \(also colloquially referenced as "tab bars"\).

### Static Notebook <a id="static-notebook"></a>

![Static Notebook](https://elementary.io/images/docs/human-interface-guidelines/notebooks/static-notebook.png)

A Static Notebook is a small set of unchanging tabs, commonly seen in preferences or settings screens. The tabs appear as linked buttons centered at the top of the content area. A Static Notebook should typically contain two to five tabs.

### Dynamic Notebook <a id="dynamic-notebook"></a>

![Dynamic Notebook](https://elementary.io/images/docs/human-interface-guidelines/notebooks/dynamic-notebook.png)

A Dynamic Notebook is a way for an app to provide user-manageable tabbing functionality, commonly seen in web browsers. The tabs appear attached to the toolbar on their own tab bar above the relevant content. Tabs are able to be rearranged and closed, and a "new tab" button is at the start of the notebook widget.

## Toasts <a id="toasts"></a>

A Toast is an ephmeral in-app notification overlaid on top of other content used to affirm user action and/or provide a time-sensitive contextual action. After a short timeout, a Toast is automatically dismissed. Because they are ephemeral, Toasts should follow recent user action when the user is expected to engaging with the app. A Toast can offer a contextual action that can be triggered as long as the Toast is visible. A Toast includes a manual dismiss button, e.g. if it is likely to be covering content the user may be more interested in.

A Toast's title should be phrased to affirm user action, even when providing an [Undo](ui-toolkit-elements.md#always-provide-an-undo) action. For example, "Foo was deleted", while offering an action to "Undo".

Use a Toast to affirm user action and provide a contextual action, like confirming content was deleted while offering an undo, or informing the user that an in-app process completed while offering a button to navigate to the result of the process. If you provide a Toast in your app to notify of the result of a background process, consider only using a Toast if your app window is focused wihle throwing a system notification if the window is not focused.

