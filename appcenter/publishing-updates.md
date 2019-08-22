# Publishing Updates

After your app has been published in AppCenter, you can issue a new update at any time by [creating a new release](https://github.com/elementary/houston/wiki/Submission-Process). This will begin the submission process again for your app.

## Updating Screenshots

Screenshots are uploaded to the AppCenter screenshot server at publication time. The URL referenced in your AppData.xml file will be automatically replaced during publication and any changes you make to images hosted at the remote URL will not be reflected in AppCenter.

{% hint style="info" %}
If you change the UI of your app in an update, you **must** update your screenshots
{% endhint %}

## Release Descriptions

You are highly encouraged to add release descriptions \(aka Changelogs\) to your appdata.xml file when publishing an update. For more information on formatting, see the `<releases/>` tag on [this page](https://www.freedesktop.org/software/appstream/docs/chap-Metadata.html#sect-Metadata-GenericComponent).

