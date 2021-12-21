# Publishing Updates

After your app has been published in AppCenter, you can issue a new update at any time by [creating a new release](https://github.com/elementary/houston/wiki/Submission-Process). This will begin the submission process again for your app.

## Updating Screenshots

Screenshots are uploaded to the AppCenter screenshot server at publication time. The URL referenced in your MetaInfo file will be automatically replaced during publication and any changes you make to images hosted at the remote URL will not be reflected in AppCenter.

{% hint style="info" %}
If you change the UI of your app in an update, you **must** update your screenshots
{% endhint %}

## Release Descriptions

You must add accurate release descriptions to your metainfo.xml file when publishing an update. For more information on formatting, see the [FreeDesktop.org AppStream documentation](https://www.freedesktop.org/software/appstream/docs/chap-Metadata.html#sect-Metadata-GenericComponent) for the `<releases />` tag. The version in your release should match the latest version submitted to AppCenter.

Release descriptions should be accurate, concise, and written for a typical user of your app—focus on user-visible and user-understandable changes, and simplify or omit technical "under the hood" changes. For example, a good release description might be:

> New features:
>
> * Support for WebP images
> * Option to maintain aspect ratio when cropping
>
> Other improvements:
>
> * Main view now loads twice as fast
> * Updated French translations
> * Other code cleaning

…while a poor one would be:

> * Fix bug \#1234; rewrote method to be async
> * Linted all the code, removed trailing whitespace
> * Refactored utils
> * Updated VAPIs

Previous release descriptions can remain in your MetaInfo file in perpetuity; AppCenter may use these to display cumulative descriptions between an older installed version and the latest version.

## GitHub User/Org Name Changes

AppCenter **does not support changing your GitHub username or organization name**, as we rely on the unique RDNN of your app to avoid conflicts—and there is not an automated way to change the RDNN throughout the whole stack from AppCenter Dashboard down to the .deb files built and hosted in the AppCenter repository. **If you change your GitHub username or organization name associated with your app, you will no longer be able to publish updates to your app** without changing its name and branding. If you have ended up in this situation, you will receive an issue filed against your app with more details and with some options.

