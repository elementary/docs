# Continuous Integration

When you submit your app for publishing in AppCenter, a suite of automated tests is run to check for things like metadata validity, file naming and installation, and successful package builds. Houston CI is a version of this test suite that can be run continuously on your GitHub projects using [Travis CI](https://travis-ci.org/).

Setting up Houston CI for your app can save you time and give you reassurance that when you submit your app for publishing it will pass the automated testing phase in AppCenter Dashboard. Keep in mind however, that there is also a human review portion of the submission process, so an app that passes Houston CI may still need fixing before being published in AppCenter.

## Configuring Travis

If you've never used Travis CI before, refer to their [Getting Started guide](https://docs.travis-ci.com/user/getting-started/).

### Standard .travis.yml

In order to build your app using Houston CI, Travis requires a configuration file called `.travis.yml` in the root directory of your repo with the following contents:

```yaml
---

language: node_js

node_js:
  - 10.17.0

sudo: required

services:
  - docker

addons:
  apt:
    sources:
      - ubuntu-toolchain-r-test
    packages:
      - libstdc++-5-dev

install:
  - npm i -g @elementaryos/houston

script:
  - houston ci
```

### Testing with Houston CI

Depending on how you've configured Travis, Houston CI will automatically run its suite of tests on your master branch as well as any branches submitted via pull request.

