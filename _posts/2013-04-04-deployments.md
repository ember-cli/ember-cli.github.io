---
layout: post
title: "Deployments"
permalink: deployments
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-04-deployments.md"
---

You can easily deploy your Ember CLI application to a number of places using
[ember-cli-deploy](http://ember-cli-deploy.com/). Or you can follow some of the
recipes below.

### Heroku

Prerequistes:

- An Ember CLI application
- [Heroku Account](https://www.heroku.com)
- [Heroku Toolbelt](https://toolbelt.heroku.com)

Official Buildpack and instructions can be found [here](https://github.com/heroku/heroku-buildpack-ember-cli).

### Azure

Continuous deployment with [Azure Websites](http://www.azure.com) is enabled
through Microsoft's module
[ember-cli-azure-deploy](https://github.com/felixrieseberg/ember-cli-azure-deploy).
The installation is simple - just run the following commands in your Ember CLI
app's root directory:

{% highlight bash %}
npm install --save-dev -g ember-cli-azure-deploy
azure-deploy init
{% endhighlight %}

Next, set up your Azure Website's source control to point to your repo -
[either via GitHub, BitBucket, VSO or any of the other available
options](http://azure.microsoft.com/en-us/documentation/articles/web-sites-publish-source-control/#Step4).
As soon as you push a new commit to your repository, Azure Websites will
automatically run `ember build` and deploy the contents of the created `dist`
directory to your website's `wwwroot`.

### Firebase

To deploy your Ember CLI application to Firebase, you'll first need to enable
hosting from your Firebase's Dashboard. Then, install the [Firebase
Tools](https://github.com/firebase/firebase-tools):

{% highlight bash %}
npm install -g firebase-tools
{% endhighlight %}

You can then configure your application for deployment by running the following
in your app's root directory and following the prompts:

{% highlight bash %}
firebase init
{% endhighlight %}

Finally, to deploy your application, run:

{% highlight bash %}
firebase deploy
{% endhighlight %}

For more configuration options, check out Firebase's [Hosting
Guide](https://www.firebase.com/docs/hosting/guide/).

### History API and Root URL

If you are deploying the app to somewhere other than the root URL (`/`),
you will need to configure the value of `rootURL` in `config/environment.js`.
This is required for the History API, and thus also the Router, to function correctly.

For example

{% highlight javascript %}
// config/environment.js
if (environment === 'production') {
  ENV.rootURL = '/path/to/ember/app/';
}
{% endhighlight %}

This will also be used as a prefix for assets, eg `/path/to/ember/app/assets/vendor.js`. However when
building for production the value of `prepend` for `fingerprint` will be used used instead. So for

{% highlight bash %}
ember build --prod
{% endhighlight %}

with

{% highlight javascript %}
// ember-cli-build.js
module.exports = function(defaults) {
  var app = new EmberApp(defaults, {
    // Add options here
    fingerprint: {
      prepend: 'https://cdn.example.com/'
    }
  });
{% endhighlight %}

the asset URLs will not use `rootURL` and will be like
`https://cdn.example.com/assets/vendor-3b1b39893d8e34a6d0bd44095afcd5c4.js`.

As of version 2.7, `baseURL` is deprecated and `rootURL` should be used
instead. See this [blog post](http://emberjs.com/blog/2016/04/28/baseURL.html)
for more details.

<a id="deploy-content-security-policy"></a>

### Content security policy


For those interested in enhanced security for their web application, they
should consider the setting up a content-security policy even for development.
That way security violations can be discovered immediately, rather then in
production.

For more information, see the [ember-cli-content-security-policy readme.]( https://github.com/rwjblue/ember-cli-content-security-policy)

### Deploying an HTTPS server using Nginx on a Unix/Linux/MacOSx machine

The following is a simple deployment with https using nginx.  Http just
redirects to the https server here.  Don't forget to include your ssl keys in
your config.

Before deployment make sure you run this command to populate the dist directory:

{% highlight bash %}
ember build --environment="production"
{% endhighlight %}

#### File: nginx.conf

    ## Nginx Production Https Ember Server Configuration

    ## https site##
    server {
        listen      443 default;
        server_name <your-server-name>;
        #root        /usr/share/nginx/html;
        root        <root path to an ember /dist directory>;
        index       index.html index.htm;

        # log files
        access_log  /var/log/nginx/<your-server-name>.access.log;
        error_log   /var/log/nginx/<your-server-name>.error.log;

        # ssl files
        ssl on;
        keepalive_timeout   60;

        # include information on SSL keys, cert, protocols and ciphers
        # SSLLabs.com is a great resource for this, along with testing
        # your SSL configuration: https://www.ssllabs.com/projects/documentation/

        # proxy buffers
        proxy_buffers 16 64k;
        proxy_buffer_size 128k;

        ## default location ##
        location / {
            include /etc/nginx/mime.types;
            try_files $uri $uri/ /index.html?/$request_uri;
        }

    }

    ## http redirects to https ##
    server {
        listen      80;
        server_name <your-server-name>;

        # Strict Transport Security
        add_header Strict-Transport-Security max-age=2592000;
        rewrite ^/.*$ https://$host$request_uri? permanent;
    }
