# Text

Although elementary OS primarily uses graphics as a means of interaction, text is also widely used for things like button labels, tooltips, menu items, dialog messages, and more. Using text consistently and clearly both in terminology and format is an extremely important part of designing your app and helps add to the overall cohesiveness of the elementary OS platform.

* **Writing Style**. Keep your text understandable and consistent with the rest of elementary OS.
* **Language**. Keep other languages in mind.
* **Capitalization**. What, when, and where to capitalize.
* **Punctuation**. Avoid common mistakes and follow convention.
* **Using Ellipsis**. When, where, and why to use ellipsis.
* **Naming Menu Items**. Keep your menus clear and consistent.

## Writing Style <a id="writing-style"></a>

Use the following rules to keep your text understandable and consistent:

### Be Brief <a id="be-brief"></a>

Don't give the user a bunch of text to read; a lengthy sentence can appear daunting and may discourage users from actually reading your messaging. Instead, provide the user with short and concise text.

* **Bad**: It doesn't look like you have any music in your library. You can use Music to organize your music, add more, and listen to the music you already have. To get started, click on the "Add" button, then follow the prompts. Once you're done, your Music will be displayed here.
* **Better**: Get Some Tunes. Music can't seem to find your contacts. \[Buttons to import or create contacts\]

### Think Simple <a id="think-simple"></a>

Assume the user is intelligent, but not technical. Avoid long, uncommon words and focus on using common, simple verbs, nouns, and adjectives. Never use technical jargon.

* **Bad**:37 audio format files have been successfully imported into the songs database.
* **Better**: Successfully added 37 songs.

### Get To The Bottom Line <a id="get-to-the-bottom-line"></a>

Put the most important information at the beginning of your text. If the user stops reading, they'll still have what they need in mind.

* **Bad**: Your network connection might be down because Lingo could not find any definitions. Check it and try again.
* **Better**: No definition found. Check your network connection and try again.

### Don't Repeat Yourself <a id="dont-repeat-yourself"></a>

Repetition can be annoying and adds unnecessary length to your messaging.

* **Bad**: Your email address, johndoe@email.com, has been added. To access johndoe@email.com, click on "johndoe@email.com" above.
* **Better**: Your email address has been added. To access it, click "johndoe@email.com" above.

### Use Visual Hierarchy <a id="use-visual-hierarchy"></a>

Visual hierarchy aids users in reading and comprehending your text as well as knowing what is most important. Use headings and other text styles appropriately.

* **Bad**:

  No files are open. Open a file to begin editing.

* **Better**:

  **No files are open.**

  Open a file to begin editing.

## Language <a id="language"></a>

While much of elementary OS is developed in English, there are many users who do not know English or prefer their native language. While putting text into your app, keep the following in mind:

* **Make it translatable.** Always, always make your text translatable using the built-in methods. It can't be translated if you don't make it translatable. Include punctuation in translatable strings.
* **Avoid culture-specific references.** Remember that users of another language are going to be using your app. Specific metaphors or references will likely be lost on or downright confusing to other those users. Instead, use universal text.
* **Keep differences in mind.** Remember that other languages and cultures often use different currencies, date formats, punctuation, and more. Always keep these things in mind when developing your app's text, and use the system-provided methods for translating these items when possible.
* **Right-to-left.** It's easy to forget about right-to-left \(RTL\) languages if you're so used to using left-to-right. Make sure your app still works well when used with RTL layouts, and always use RTL-compatible widgets when developing your app.

## Capitalization <a id="capitalization"></a>

All textual user interface items, including labels, buttons, and menus, should use one of two capitalization styles: sentence case or title case.

### Sentence Case <a id="sentence-case"></a>

Sentence case means you capitalize like in a standard sentence or phrase.

Only the first letter of the phrase and the first letter of proper nouns are capitalized. Used for labels and descriptive text.

* ex. "Always open MPEG files with Marlin" next to a checkbox.
* ex. "Read news feeds" for the description of an RSS reader.
* ex. "This folder is empty" in a file manager.

### Title Case <a id="title-case"></a>

Title case means you capitalize like a book or article title. In elementary OS, we follow the [AP Style](https://www.bkacontent.com/how-to-correctly-use-apa-style-title-case/).

* Capitalize the first and last words.
* Capitalize all nouns, pronouns, adjectives, verbs, adverbs, and subordinate conjunctions \(as, because, although\).
* Capitalize all words that are more than three letters long.
* Don’t capitalize articles, prepositions or conjunctions that are three or fewer letters long.

Used for titles, buttons, menus, and most other widgets.

* ex. "Open File" title in a dialog.
* ex. "Delete 13 Songs" on a button.
* ex. "Search Contacts" in a search bar.
* ex. "No Videos Open" on a welcome screen.

### Notes/Exceptions <a id="notes-exceptions"></a>

Proper nouns should always be capitalized properly; this means that, for example, Google should always be shown as "Google," and MPEG should always be shown as "MPEG." One major exception is "elementary." It should follow the brand guidelines and always appear lowercase, even at the start of a sentence. If you're unsure how a certain pronoun should be officially capitalized, refer to the documentation of the pronoun in question.

Keep in mind these are the rules elementary OS follows for English; capitalization for other languages should follow the conventions of those languages.

## Punctuation <a id="punctuation"></a>

Proper typography is important throughout elementary OS. Not just for consistency within the OS, but for following proper convention and presenting ourselves as a serious, professional platform.

### Terminating Punctuation <a id="terminating-punctuation"></a>

Whether or not to use terminating punctuation \(like a period `.` in English\) depends on context. For secondary labels in [dialogs](text.md#dialogs), use terminating punctuation. For single-sentence copy in tooltips and other clarifying contexts, avoid terminating punctuation.

For multi-sentence copy in clarifying contexts, use standard terminating punctuation. If there are single-sentence labels in the same context \(alongside multi-sentence labels\), terminating punctuation may be used for consistency.

For questions, always include terminating punctuation \(i.e. a `?` in English\).

### Prevent Common Mistakes <a id="prevent-common-mistakes"></a>

* Only use **one space** after a period.
  * i.e. “This is a sentence. This is another sentence.”
* Use a proper ellipsis character \(…\) rather than three dots.
  * Use `\u2026` in your code.
  * See [Using Ellipses](text.md#using-ellipses) for usage details.
* For quotes, use proper **left and right double quotation characters** \(“ and ”\).
  * Use `\u201C` and `\u201D` in your code.
* In contractions and possession, use a **right single quotation mark** \(’\) as an apostrophe.
  * Use `\u2019` in your code.
* Use **real math symbols** for subtraction \(−\), multiplication \(×\), and division \(÷\) signs.
  * Use `\u2212`,  `\u00D7`,  and `\u00F7` in your code.
* Use correct **copyright symbols** \(©\).
  * Use `\u0169` in your code.
* Use **superscripts** where needed \(e.g. 1st\).

### Hyphens & Dashes <a id="hyphens-and-dashes"></a>

#### Hyphen \(-\) <a id="hyphen"></a>

Use `\u2010` in code. Used for:

* Joining words \(e.g. non-breaking\).
  * _If the word cannot be split along lines \(e.g. UTF-8\), use a non-breaking hyphen \(- `\u2011`\)._

#### En Dash \(–\) <a id="en-dashe"></a>

Use `\u2013` in code. Used for:

* Number ranges \(e.g. 5–12\). _Do not put a space on either side._

#### Em Dash \(—\) <a id="em-dashe"></a>

Use `\u2014` in code. Used for:

* Interjections \(e.g. the party—which was scheduled for Thursday—was delayed\).
  * _Do not put a space on either side._
* Quote offsets.
  * _New line, space to the right._

If in doubt, refer to [Butterick's Practical Typography](https://practicaltypography.com/).

These rules apply to the English language; other languages may have their own conventions which should be followed by translators.

## Using Ellipses <a id="using-ellipses"></a>

The ellipsis character \(…\) is used in the interface for two primary reasons: informing the user of an additional required information and letting the user know text has been shortened.

### Additional Information <a id="additional-information"></a>

An ellipsis should be used to let a user know that more information or a further action is required before their action can be performed. Usually this means that the user should expect a new interface element to appear such as a new window, dialog, toolbar, etc in which they must enter more information or make a selection before completing the intended action. This is an important distinction because a user should typically expect an instant action from buttons and menu items while this prepares them for an alternate behavior. More specifically, an ellipsis should be used when the associated action:

* **Requires specific input from the user.** For example, Find, Open, and Print commands all use ellipses because the user must select or input the item to find, open, or print. An easy way to remember this is to think of a question requiring and answer: Find what? Open what? Print what?
* **Is performed in a new window or dialog.** For example, Preferences, Report a Problem, and Customize Toolbar all use ellipses because they open a new window \(sometimes another app\) or dialog in which the user makes a selection or inputs other information. Consider that the absence of an ellipsis implies the app will handle the action immediately. This means that the app will automatically generate a report or customize its own toolbar. Using an ellipsis makes the important distinction that the user will be writing the report or selecting which toolbar items to show.
* **Warns the user of a potentially dangerous action.** For example, Log Out, Restart, and Shut Down all use ellipses because they display an alert that asks the user to confirm or cancel a potentially harmful action. Again, this is the user clicking a button or menu item that requires them to input more information or make a selection before the action is completed.

### Shortened Text <a id="shortened-text"></a>

Ellipses should be used when shortening text that cannot fit in any specific place. For example, if a playlist's name is longer than the space available in the sidebar, truncate it and use an ellipsis to signify its truncation. There are two ways to use an ellipsis when shortening text:

* End truncation. If the important or distinctive text is at the beginning of the string, truncate it at the end and append an ellipsis.
* Middle truncation. When the end of the text is more important or distinctive, truncate the text in the middle and replace the truncated text with an ellipsis.

If you're unsure, it's best to use middle truncation, as it keeps both the beginning and end of the string intact. It's also important that you do not ship your app with any truncated text; truncation should only be the result of a user action such as resizing a sidebar or entering custom text.

### When Not to Use Ellipsis <a id="when-not-to-use-ellipsis"></a>

* In an entry's placeholder text. An ellipsis is not necessary here and adds no value.
* For a submenu item. The right arrow already indicates that this entry spawns a submenu and is not an action.

Be sure to use the actual ellipsis character \(…\) rather than three consecutive period \(.\) characters.

## Naming Menu Items

Menu items should have names that are either actions or locations, never descriptions. Make sure menu items are concise, but also fully describe the action that will be performed when they are clicked.

"Find in Page..." is acceptable as it clearly describes the action that will be performed when the item is clicked. "Software Up to Date" is not acceptable. What happens if I click this item? Where will it take me? What will it do? The outcome is uncertain.

