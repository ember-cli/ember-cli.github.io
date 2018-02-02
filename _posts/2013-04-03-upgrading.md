---
layout: post
title: "Upgrading"
permalink: upgrading
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-03-upgrading.md"
---

### Upgrading an Ember CLI App

To upgrade an Ember CLI app, first delete temporary folders by running `rm -rf node_modules dist tmp`.
Then, if you are using yarn, run:

```bash
yarn upgrade ember-cli
```

If you are using npm, run:

```bash
npm install --save-dev ember-cli
```

After upgrading your project's dependencies, run `ember init` to apply the blueprint changes.
Please follow the prompts, and review all changes. The most common source of upgrade pain is missing changes in this step, you can press `d` to see a diff when reviewing.

Steps to upgrade to the latest version of Ember CLI are also included with the
[release notes for each release](https://github.com/ember-cli/ember-cli/releases).
