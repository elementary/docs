# Icons

The last thing we need for a minimum-viable-app is to provide app icons. Apps on elementary OS provide icons hinted in 6 sizes: 16px, 24px, 32px, 48px, 64px, and 128px. These icons will be shown in many places throughout the system, such as those listed below:

| Size  | AppCenter | Applications Menu | Dock    | Menus & Popovers  | Multitasking | Notifications |
| :-----| :---------| :---------------- | :------ | :---------------- | :----------- | :------------ |
| 16px  | ✖️ No     | ✖️ No             | ✔️ Yes  | ✔️ Yes            | ✔️ Yes       | ✖️ No         |
| 24px  | ✖️ No     | ✖️ No             | ✖️ No   | ✖️ No             | ✔️ Yes       | ✔️ Yes        |
| 32px  | ✖️ No     | ✔️ Yes            | ✔️ Yes  | ✔️ Yes            | ✔️ Yes       | ✖️ No         |
| 48px  | ✔️ Yes    | ✖️ No             | ✔️ Yes  | ✖️ No             | ✔️ Yes       | ✔️ Yes        |
| 64px  | ✔️ Yes    | ✔️ Yes            | ✔️ Yes  | ✖️ No             | ✔️ Yes       | ✖️ No         |
| 128px | ✔️ Yes    | ✖️ No             | ✖️ No   | ✖️ No             | ✖️ No        | ✖️ No         |


{% hint style="info" %}
To help you provide the necessary sizes—and for this tutorial—Micah Ilbery maintains an icon template project [here on GitHub](https://github.com/micahilbery/elementary-icon-templates)
{% endhint %}

Place your icons in the data directory and name them after their pixel sizes, such as`32.svg`, `64.svg`, etc. The file structure should look like this:

```
hello-again
    data
        16.svg
        24.svg
        32.svg
        48.svg
        64.svg
        128.svg
```

Now that you have icon files in the data directory, add the following lines to the end of `meson.build` to install them .

```coffeescript
# Install our icons in all the required sizes
icon_sizes = ['16', '24,' '32', '48', '64', '128']

foreach i : icon_sizes
    install_data(
        'data' / i + '.svg',
        install_dir: get_option('datadir') / 'icons' / 'hicolor' / i + 'x' + i / 'apps',
        rename: meson.project_name() + '.svg'
    )
    install_data(
        'data' / i + '.svg',
        install_dir: get_option('datadir') / 'icons' / 'hicolor' / i + 'x' + i + '@2' / 'apps',
        rename: meson.project_name() + '.svg'
    )
endforeach
```

You'll notice the section for installing app icons is a little more complicated than installing other files. In this example, we're providing SVG icons in all of the required sizes for AppCenter and, since we're using SVG, we're installing them for both LoDPI and HiDPI. If you're providing PNG icons instead, you'll need to tweak this part a bit to handle assets exported for use on HiDPI displays.

{% hint style="info" %}
For more information about creating and hinting icons, check out the [Human Interface Guidelines](https://docs.elementary.io/hig/reference/iconography#size)
{% endhint %}
