# Translations

Now that you've learned about Meson, the next step is to make your app able to be translated to different languages. The first thing you need to know is how to mark strings in your code as translatable. Here's an example:

```vala
stdout.printf ("Not Translatable string");
stdout.printf (_("Translatable string!"));

string normal = "Another non-translatable string";
string translated = _("Another translatable string");
```

See the difference? We marked the string as translatable by adding `_()` around it. Go back to your project and make all your strings translatable by adding `_()`.

Now we have to make some changes to our Meson build system and add a couple new files to describe which files we want to translate and which languages we want to translate into.

1. Open up your "meson.build" file and add these lines below your project declaration:

   ```coffeescript
   # Include the translations module
   i18n = import('i18n')

   # Set our translation domain
   add_global_arguments('-DGETTEXT_PACKAGE="@0@"'.format (meson.project_name()), language:'c')
   ```

2. Remove the lines that install your .desktop and metainfo files and replace them with the following:

   ```coffeescript
   #Translate and install our .desktop file
   i18n.merge_file(
       input: 'data' / 'hello-again.desktop.in',
       output: meson.project_name() + '.desktop',
       po_dir: meson.source_root() / 'po',
       type: 'desktop',
       install: true,
       install_dir: get_option('datadir') / 'applications'
   )

   #Translate and install our .metainfo file
   i18n.merge_file(
       input: 'data' / 'hello-again.metainfo.xml.in',
       output: meson.project_name() + '.metainfo.xml',
       po_dir: meson.source_root() / 'po',
       install: true,
       install_dir: get_option('datadir') / 'metainfo'
   )
   ```

   The `merge_file` method combines translating and installing files, similarly to how the `executable` method combines building and installing your app. You might have noticed that this method has both an `input` argument and an `output` argument. We can use this instead of the `rename` argument from the `install_data` method.

3. Still in this file, add the following as the last line:

   ```coffeescript
   subdir('po')
   ```

4. You might have noticed in step 2 that the `merge_file` method has an `input` and `output`. We're going to append the additional extension `.in` to our .desktop and .metainfo.xml files so that this method can take the untranslated files and produce translated files with the correct names.

   ```bash
   git mv data/hello-again.desktop data/hello-again.desktop.in
   git mv data/hello-again.metainfo.xml data/hello-again.metainfo.xml.in
   ```

   We use the `git mv` command here instead of renaming in the file manager or with `mv` so that `git` can keep track of the file rename as part of our revision history.

5. Now, Create a directory named "po" in the root folder of your project. Inside of your po directory you will need to create another "meson.build" file. This time, its contents will be:

   ```coffeescript
   i18n.gettext(meson.project_name(),
       args: '--directory=' + meson.source_root(),
       preset: 'glib'
   )
   ```

6. Inside of "po" create another file called "POTFILES" that will contain paths to all of the files you want to translate. For us, this looks like:

   ```text
   src/Application.vala
   data/hello-again.desktop.in
   data/hello-again.metainfo.xml.in
   ```

7. We have one more file to create in the "po" directory. This file will be named "LINGUAS" and it should contain the two-letter language codes for all languages you want to provide translations for. As an example, let's add German and Spanish

   ```text
   de
   es
   ```

8. Now it's time to go back to your build directory and generate some new files using Terminal! The first one is our translation template or `.pot` file:

   ```bash
   ninja io.github.yourusername.yourrepositoryname-pot
   ```

   After running this command you should notice a new file in the po directory containing all of the translatable strings for your app.

9. Now we can use this template file to generate translation files for each of the languages we listed in the LINGUAS file with the following command:

   ```bash
   ninja io.github.yourusername.yourrepositoryname-update-po
   ```

   You should notice two new files in your po directory called `de.po` and `es.po`. These files are now ready for translators to localize your app!

10. Last step. Don't forget to add all of the new files we created in the po directory to git:

    ```bash
    git add src/Application.vala meson.build po/ data/
    git commit -am "Add translations"
    git push
    ```

That's it! Your app is now fully ready to be translated. Remember that each time you add new translatable strings or change old ones, you should regenerate your .pot and po files using the `-pot` and `-update-po` build targets from the previous two steps. If you want to support more languages, just list them in the LINGUAS file and generate the new po file with the `-update-po`target. Don't forget to add any new po files to git!

{% hint style="info" %}
If you're having trouble, you can view the full example code [here on GitHub](https://github.com/vala-lang/examples/tree/localization)
{% endhint %}

## Translators Comments

Sometimes detailed descriptions in the context of translatable strings are necessary for disambiguation or to help in the creation of accurate translations. For these situations use `/// TRANSLATORS:` comments.

```vala
/// TRANSLATORS: The first %s is search term, the second is the name of default browser
title = _("Search for %s in %s").printf (query, get_default_browser_name ());
```
