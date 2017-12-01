---
layout: post
title: "Deployments"
permalink: deployments
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-04-deployments.md"
---

You can easily deploy your Ember CLI application to a number of places using
[ember-cli-deploy](http://ember-cli-deploy.com/). 

Additional Examples:

* https://www.heroku.com/emberjs


### History API and Root URL

If you are deploying the app to somewhere other than the `rootURL` (`/`),
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
building for production the value of `prepend` for `fingerprint` will be used instead. So for

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

the asset URLs will not use `rootURL` and will instead be:
`https://cdn.example.com/assets/vendor-3b1b39893d8e34a6d0bd44095afcd5c4.js`.

As of version 2.7, `baseURL` is deprecated and `rootURL` should be used
instead. See this [blog post](http://emberjs.com/blog/2016/04/28/baseURL.html)
for more details.

### Content Security Policy

For those interested in enhanced security for their web application, they
should consider the setting up a content-security policy even for development.
That way security violations can be discovered immediately, rather than in
production.

For more information, see the [`ember-cli-content-security-policy` README.]( https://github.com/rwjblue/ember-cli-content-security-policy)

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
