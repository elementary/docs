---
description: The submission process for AppCenter, broken down into a few short steps
---

# Submission Process

{% hint style="info" %}
Please make sure to review [the publishing requirements](publishing-requirements.md) before submitting your app
{% endhint %}

## **Create a New Release**

Before you can publish your app, you'll need to [release it on GitHub](https://help.github.com/articles/creating-releases/). It is important that you use a [Semantic Versioning Number](http://semver.org/) without a pre-release tag when releasing.

## **Submit for Review**

{% hint style="danger" %}
It is important to [make changes to your app's monetization status](monetizing-your-app.md) before completing this step
{% endhint %}

To submit your app to AppCenter, you'll need to create a new JSON file describing your app release in the AppCenter Reviews repository on Github:

{% embed url="https://github.com/elementary/appcenter-reviews/new/main/applications" %}
Select this link to automatically open a new file in the correct location
{% endembed %}

Name the file using your app's id with the `json` file extension. Fill in the contents of the file with the link to your git repository, the commit hash from your latest release, and the version number of that release:

{% code title="io.github.myteam.myapp.json" %}
```json
{
  "source": "https://github.com/danirabbit/harvey.git",
  "commit": "45dd52660c0069207fa4b0c1fcf7ac6d7157d34b",
  "version": "1.0.1"
}
```
{% endcode %}

The `source` field needs to be a publicly accessible Git repository. The `commit` field is the full Git sha for the release you're submitting. And the `version` field needs to be a [SemVer](http://semver.org/) tag.

Once you've submitted the new file as a pull request to the AppCenter Reviews repository, your app will now be in the review queue!

## Wait for Testing

All apps and updates in the review queue are listed as [open pull requests](https://github.com/elementary/appcenter-reviews/pulls?q=is%3Apr+is%3Aopen+sort%3Acreated-desc). Once a pull request is approved and merged by a reviewer, that app will be published to the [AppCenter Flatpak repository](https://flatpak.elementary.io/). Depending on demand, this may take several days.

## Resolve **Issues**

Your app will go through an automated build process when it has been accepted by a reviewer. It will need to pass the `Parse` and `Build` steps listed in GitHub CI at the bottom of the pull request. If either of those steps fail, please inspect the build log to resolve issues.

If any questions or requested changes come up during the review process, the reviewer will use the conversation on the pull request and may mark it as "Changes Requested". You will need to correct these issues, create a new release of your app, and update the pull request with your new release.

The reviewer may also make some suggestions for best practices, inform you of new features, or link you to documentation.

## Available on AppCenter

When your app passes automated testing and human review, it will become available in AppCenter in elementary OS.

## Publishing Updates

If you wish to [publish an update](publishing-updates.md), you can restart the submission process by creating a new release on GitHub. Then navigate to the [AppCenter Reviews Github repository](https://github.com/elementary/appcenter-reviews/tree/main/applications), modify your app's json file with the latest commit hash and release tag, and submit that change as a pull request to begin a new review process.
