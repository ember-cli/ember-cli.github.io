---
layout: post
title: "Windows"
category: user-guide
permalink: windows
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-02-practices-windows.md"
---

### Windows

Windows Vista and newer windows versions are fully supported.

To get started ensure the following dependencies are installed:

* Node.js - [https://nodejs.org/en/](https://nodejs.org/en/)
* Git - [https://git-scm.com/](https://git-scm.com/)
* Phantom.js - [http://phantomjs.org/](http://phantomjs.org/)

### Performance

Although supported, Windows performance, atleast by default, isn't as good as
on Linux or MacOS. On a positive note, this story continues to improve. Both
Microsoft, and the ember-cli team continue to work to improve these developer
ergonomics.

#### What causes the build slowdown?

The two primary reasons are:

* Lack of enabled-by-default symlinks
* Generally slower FS operations on NTFS

#### For the best possible windows experience:

* Windows 10, insiders release + development mode enabled (Symlinks enabled by
  default) [Details from
  Microsoft](https://blogs.windows.com/buildingapps/2016/12/02/symlinks-windows-10/)
* or, Windows Subsystem Linux [Installation
  Guide](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide)

#### Improving your windows experience:

Ensure Search and Defender ignore your projects tmp directory:

{% highlight bash %}
npm install --save-dev ember-cli-windows-addon
{% endhighlight %}

Then, to start the automatic configuration, run:

{% highlight bash %}
ember windows
{% endhighlight %}

[Readmore about this from the Microsoft DX Open Source team](http://felixrieseberg.com/improved-ember-cli-performance-with-windows/)

#### Enabling symlinks

In order to create symlinks the account running Ember CLI must have the
`SeCreateSymbolicLinkPrivilege`. Users in the Administrators group have this
permission already. However if UAC (User Access Control) is enabled users in
the Administrators group must run their shell using `Run As Administrator`.
This is because UAC strips away certain permissions from the Administrators
group, including `SeCreateSymbolicLinkPrivilege`.

![Run As Administrator]({{ site.url }}/assets/images/common-issues/run-as-admin.png)

If the user account is not part of the Administrators group you will need to
add the `SeCreateSymbolicLinkPrivilege` in order to allow the creation of
symlinks. To do this open the `Local Security Policy` by typing Local Security
Policy in the Run Box.

Under `Local Policies` -> `User Rights Assignment` find the `Create symbolic
links` policy and double click it to add a new user or group. Once you add your
user or group has been added your user should be able to create symlinks. Keep
in mind if your user is part of the Administrators group and UAC is enabled you
will still need to start your shell using `Run as Administrator`.

![Enabling Symlinks]({{ site.url }}/assets/images/common-issues/enabling-symlinks.png)

#### Issues With npm: EEXISTS, Path too Long, etc

There were always two major issues with running Node.js on Windows: First and
foremost, the operating system maintains a maximum length for path names, which
clashes with Node's traditional way of nesting modules in node_modules. The
second issue was a bit more subtle: The npm installer had a set of steps it
executed for each package and it would immediately start executing them as soon
as it decided to act on a package, resulting in hard-to-debug race conditions.

Npm@3 is a nearly complete rewrite of npm, fixing both issues. Windows users of
Ember Cli might want to make the switch to npm@3 early, to benefit from its
flat module installation (solving any issues involving long path names) as well
as its multi-stage installer.

The new version is currently still in beta - and while performing extremely
well so far, you might still run into issues. To easily up- and downgrade
between versions, use [Microsoft's npm-windows-upgrade
tool](https://github.com/felixrieseberg/npm-windows-upgrade), which automates
the upgrade and also allows easy downgrades to older versions.
