---
layout: post
title: "Upgrading"
permalink: upgrading
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-03-upgrading.md"
---

### Upgrading an Ember CLI App

To upgrade an Ember CLI app use `ember-cli-update`, install instructions can be found [here](https://github.com/ember-cli/ember-cli-update#installation).

When it's done, if you installed `ember-cli-update` globally, run the following command inside your project directory,

ember-cli-update

or if you installed as an Ember CLI command, run

ember update

This will update your app or addon to the latest Ember CLI version. It does this by fetching the latest version and comparing it to your project's Ember CLI version. It then applies a diff of the changes from the latest version to your project. It will only modify the files if there are changes between your project's version and the latest version, and it will only change the section necessary, not the entire file.

This is different from the existing ember init command. That command tries to reset your project back to a brand new project. It removes all your changes and additions.

You will probably encounter merge conflicts, in which the default behavior is to let you resolve conflicts on your own. You can supply the --resolve-conflicts option to run your system's git merge tool if any conflicts are found.

There's some others `ember-cli-update`'s options documented [here](https://github.com/ember-cli/ember-cli-update#options).

Steps to upgrade to the latest version of Ember CLI are also included with the
[release notes for each release](https://github.com/ember-cli/ember-cli/releases).
