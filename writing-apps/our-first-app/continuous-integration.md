# Continuous Integration

Continuous integration testing (also called CI), is a way to automatically ensure that your app builds correctly and has the minimum necessary configuration to run when it's installed. Setting up CI for your app can save you time and give you reassurance that when you submit your app for publishing it will pass the automated testing phase in AppCenter Dashboard. Keep in mind however, that there is also a human review portion of the submission process, so an app that passes CI may still need fixing before being published in AppCenter. Now that you have an app with a build system, metadata files, and packaging, we can configure CI using GitHub Actions.

1. Navigate to your project's page on GitHub and select the "Actions" tab.

2. Select "set up a workflow yourself". You'll then be shown the `main.yml` file with GitHub's default CI configuration. Replace the default workflow with the following:

```yml
name: CI

# This workflow will run for any pull request or pushed commit
on: [push, pull_request]

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "flatpak"
  flatpak:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # This job runs in a special container designed for building Flatpaks for AppCenter
    container:
      image: ghcr.io/elementary/flatpak-platform/runtime:6
      options: --privileged

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so the job can access it
    - uses: actions/checkout@v2

      # Builds your flatpak manifest using the Flatpak Builder action
    - uses: bilelmoussaoui/flatpak-github-actions/flatpak-builder@v3
      with:
        # This is the name of the Bundle file we're building and can be anything you like
        bundle: MyApp.flatpak
        # This uses your app's RDNN ID
        manifest-path: com.github.yourusername.yourrepositoryname.yml

        # You can automatically run any of the tests you've created as part of this workflow
        run-tests: true

        # These lines specify the location of the elementary Runtime and Sdk
        repository-name: appcenter
        repository-url: https://flatpak.elementary.io/repo.flatpakrepo
        cache-key: "flatpak-builder-${{ github.sha }}"
```

3. Select "Start commit" in the top right corner of the page to commit this workflow to your repository.

That's it! The Flatpak CI workflow will now run on your repository for any new commits or pull requests. You'll be able to see if the build succeeded or failed, and any reasons it might have failed. Configuring this CI workflow will help guide you during the development process and ensure that everything is working as intended.

This workflow will also produce a Flatpak Bundle file using GitHub Artifacts that you can download and install with Sideload. Using this bundle file, you can test feature or bug fix branches with your team, contributors, or community before merging the proposed fix into your main git branch.

GitHub Actions can be used to configure many more types of CI or other automation. For more information, check out the [GitHub Actions website](https://github.com/features/actions).
