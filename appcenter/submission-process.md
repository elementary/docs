# Submission Process

The submission process for AppCenter is broken down into a few simple steps. The step that you're currently on is indicated by an icon inside of a colored bubble. Hovering over that bubble with your cursor will display a tooltip with more information.

## **Install the AppCenter Dashboard GitHub Integration**

Before AppCenter Dashboard can import and publish your repos, you'll need to install the [AppCenter Dashboard GitHub Integration](https://github.com/integration/appcenter).

## **Create a New Release**

{% hint style="info" %}
This step is denoted by the tag icon in a light grey bubble.
{% endhint %}

Before you can publish your app, you'll need to [release it on GitHub](https://help.github.com/articles/creating-releases/). It is important that you use a [Semantic Versioning Number](http://semver.org/) without a pre-release tag when releasing. AppCenter Dashboard will try to sanitize your version number if it is not a proper Semver, but this may not succeed or have unintended results.

## **Submit for Review**

{% hint style="info" %}
This step is denoted by an up arrow icon in a green bubble.
{% endhint %}

If a new release is available on GitHub, you'll be able to submit this release for publishing. Please make sure to review [the publishing requirements](https://github.com/elementary/houston/wiki/Before-You-Publish) before submitting your app. After clicking the button, AppCenter Dashboard will automatically import your repository and begin testing. It is important to [make changes to your app's monetization status](https://github.com/elementary/houston/wiki/Monetizing-Your-App) before completing this step.

## Wait for Testing

{% hint style="info" %}
This step is denoted by a clock, rotating arrows, or a person icon in a yellow bubble.
{% endhint %}

Depending on demand, you may have to wait while AppCenter Dashboard is running test on other apps. When space is available, AppCenter Dashboard will perform automated tests on your app. After automated testing has completed, your app will await review by a human. Depending on demand, this may take several days.

## Resolve **Issues**

{% hint style="info" %}
This status is denoted by an exclamation mark inside a triangle.
{% endhint %}

If your app fails either human or automated testing, new issues reports will be generated in your issue tracker on GitHub. You will need to correct these issues, create a new release of your app, and return to AppCenter Dashboard to begin the submission process again.

## Available on AppCenter

{% hint style="info" %}
This status is denoted by a check mark icon in a green bubble.
{% endhint %}

When your app passes automated testing and human review, it will become available in AppCenter in elementary OS. If you wish to [publish an update](https://github.com/elementary/houston/wiki/Publishing-Updates), you can restart the submission process by creating a new release on GitHub.

