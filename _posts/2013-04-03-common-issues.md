---
layout: post
title: "Common Issues"
permalink: commonissues
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-03-common-issues.md"
---

### npm package management with sudo

Installing packages such as bower with sudo powers can lead to permissions
issues and ultimately to problems installing dependencies.

For example

{% highlight bash %}
Uncaught Error: Could not find module ember/resolver loader/loader.js:42
{% endhighlight %}

can be caused by installing bower with sudo. See
[https://gist.github.com/isaacs/579814](https://gist.github.com/isaacs/579814)
for a collection of various solutions.

### Installing from behind a proxy

If you're behind a proxy, you might not be able to install because `ember-cli`
&mdash; or some of its dependencies &mdash; tries to `git clone` a `git://`
url. (In this scenario, only `http://` urls will work).

You'll probably get an error like this:

{% highlight bash %}
npm ERR! git clone git://github.com/jgable/esprima.git Cloning into bare repository '/home/<username>/.npm/_git-remotes/git-github-com-jgable-esprima-git-d221af32'...
npm ERR! git clone git://github.com/jgable/esprima.git
npm ERR! git clone git://github.com/jgable/esprima.git fatal: unable to connect to github.com:
npm ERR! git clone git://github.com/jgable/esprima.git github.com[0: 192.30.252.129]: errno=Connection timed out
npm ERR! Error: Command failed: fatal: unable to connect to github.com:
npm ERR! github.com[0: 192.30.252.129]: errno=Connection timed out
{% endhighlight %}

This is not a `ember-cli` issue _per se_, but here's a workaround. You can configure `git` to make the translation:

{% highlight bash %}
git config --global url."https://".insteadOf git://
{% endhighlight %}

### Using canary build instead of release

For ember:

    bower install ember#canary --resolution canary

For ember-data

    npm install --save-dev emberjs/data#master

### Windows Build Performance issues

see [The Windows Section](#windows) for more details.

### PhantomJS on Windows

When running tests on Windows via PhantomJS the following error can occur:

	events.js:72
	throw er; // Unhandled 'error' event
	^
	Error: spawn ENOENT
	at errnoException (child_process.js:988:11)
	at Process.ChildProcess._handle.onexit (child_process.js:779:34)

In order to fix this ensure the following is added to your `PATH`:

`C:\Users\USER_NAME\AppData\Roaming\npm\node_modules\phantomjs\lib\phantom`

### Cygwin on Windows

Node.js on Cygwin is no longer supported [more
details](https://github.com/nodejs/node/wiki/Installation#building-on-cygwin)
Rather then using Cygwin, we recommend running ember-cli natively on windows,
or via the new [Windows Subsystem
Linux](https://msdn.microsoft.com/en-us/commandline/wsl/install_guide).

### Usage with Docker

When building your own [Docker](http://docker.com) image to build ember
applications and run tests, there are a copuple of pitfalls to avoid.
* Phantomjs requires `bzip2` and `fontconfig` to already be installed.
* After installing phantomjs, you will need to manually link phantomjs to
  /usr/local/bin if that is not done by the install process.
* Testem uses the `which` command to locate phantomjs, so you must install
  `which` if it is not included in your base OS.

### Usage with Vagrant

[Vagrant](http://vagrantup.com) is a system for automatically creating and
setting up development environments that run in a virtual machine (VM).

Running your ember-cli development environment from inside of a Vagrant VM will
require some additional configuration and will carry a few caveats.

#### Ports

In order to access your ember-cli application from your desktop's web browser,
you'll have to open some forwarded ports into your VM. EmberCli uses two ports:

* For serving assets, this one defaults to `4200` but can be configured via `--port 4200`
* For live reload, this one is choosen dynamically, but can be configure via `---live-reload-port=9999`

To make vagrant development seamless these ports will need to be forwarded

{% highlight ruby %}
Vagrant.configure("2") do |config|
  # ...
  config.vm.network "forwarded_port", guest: 4200, host: 4200
  config.vm.network "forwarded_port", guest: 9999, host: 9999
end
{% endhighlight %}

#### Watched Files

The way Vagrant syncs directories between your desktop and vm may prevent file
watching from working correctly. This will prevent rebuilds, and live reloads
from working correctly. There are several work arounds:

1. Watch for changes by polling the file system via: `ember serve --watcher polling`.
2. Use [nfs for synced folders](https://docs.vagrantup.com/v2/synced-folders/nfs.html).

#### VM Setup

When setting up your VM, install ember-cli dependencies as you normally would.
Some of these dependencies (such as [broccoli-sass](#sass)) may have native
depenencies that may require recompilation. To do so run:

```
npm rebuild
```

#### Provider

The two most common Vagrant providers, VirtualBox and VMware Fusion, will both
work. However, VMware Fusion is substantially faster and will use less battery
life if you're on a laptop. As of now, VirtualBox will use 100% of a single CPU
core to poll for file system changes inside of the VM.
