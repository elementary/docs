# Publishing Updates

## Updating Screenshots

Screenshots are uploaded to the AppCenter screenshot server at publication time. The URL referenced in your MetaInfo file will be automatically replaced during publication and any changes you make to images hosted at the remote URL will not be reflected in AppCenter.

{% hint style="info" %}
If you change the UI of your app in an update, you **must** update your screenshots
{% endhint %}

## Release Notes

Release notes show up in AppCenter both on your app info page and on the updates page.

* [x] You must add an accurate `release` tag to your MetaInfo file when publishing an update.
* [x] The version in your `release` tag should match the latest version submitted to AppCenter.
* [x] Previous `release` tags can remain in your MetaInfo file forever, and we encourage it; AppCenter may use them to show changes across multiple releases of your app

### Descriptions

The contents of the `description` tag should be written for the people who use your app. Avoid technical language and developer-facing changes and focus more on what people can expect to see in this update.

#### ✅️ Good Example

{% code overflow="wrap" %}
```xml
<release version="1.1.0 date="2023-05-21">
  <description>
    <p>Now with support for WebP images and an option to maintain aspect ratio when cropping! Plus, the main view now loads twice as fast.</p>
    <p>Other improvements:</p>
    <ul>
      <li>🇫🇷️ Updated French translations</li>
      <li>⚙️ Cleaner code with improved stability</li>
    </ul>
  </description>
</release>
```
{% endcode %}

#### ❌️ Poor Example

```xml
<release version="1.1.0 date="2023-05-21">
  <description>
    <p>Fix bug #1234; rewrote method to be async</p>
    <p>Linted all the code, removed trailing whitespace</p>
    <p>Refactored utils</p>
    <p>Updated VAPIs</p>
  </description>
</release>
```

### Issues

You can also list issues from your issue tracker (e.g. GitHub) that have been resolved in this release as a way for you to close the feedback loop that began when someone opened an issue on your tracker. Issue tags will be presented as links below a release description in AppCenter. Showing that you are responsive to feedback is a powerful tool in building a community around your app.

For now only the `generic` issue type is supported, and you must provide the `url` property. The value of the tag should be the title of the issue that it links to. For example:

{% code overflow="wrap" fullWidth="false" %}
```xml
<release version="1.1.0 date="2023-05-21">
  <description>
    <p>Now with support for WebP images and an option to maintain aspect ratio when cropping! Plus, the main view now loads twice as fast.</p>
  </description>
  <issues>
    <issue url="https://github.com/myteam/myapp/issues/28">Crash when opening corrupt JPEG</issue>
    <issue url="https://github.com/myteam/myapp/issues/31">Image gets pixelated after rotating</issue>
  </issues>
</release>
```
{% endcode %}

{% hint style="info" %}
For more information on formatting releases, see the [Release Tag Specification](https://www.freedesktop.org/software/appstream/docs/sect-Metadata-Releases.html#spec-releases).
{% endhint %}

## GitHub User/Org Name Changes

AppCenter **does not support changing your GitHub username or organization name**, as we rely on the unique RDNN of your app to avoid conflicts—and there is not an automated way to change the RDNN throughout the whole stack from AppCenter Dashboard down to the .deb files built and hosted in the AppCenter repository. **If you change your GitHub username or organization name associated with your app, you will no longer be able to publish updates to your app** without changing its name and branding. If you have ended up in this situation, you will receive an issue filed against your app with more details and with some options.
