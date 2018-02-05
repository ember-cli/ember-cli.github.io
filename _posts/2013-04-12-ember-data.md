---
layout: post
title: "Using With Ember Data"
permalink: ember-data
category: user-guide
github: "https://github.com/ember-cli/ember-cli.github.io/blob/master/_posts/2013-04-12-ember-data.md"
---

The current version of Ember Data is included with Ember CLI.

Ember Data has undergone some major reboots, drastically simplifying it and
making it easier to use with the Ember resolver. Here's some tips for using it
within Ember CLI.

To use ember-cli without Ember Data remove the dependency from package.json
(the same applies for [ic-ajax](https://github.com/rwjblue/ember-cli-ic-ajax))

  `npm rm ember-data --save-dev`


### Models

Models are critical in any dynamic web application. Ember Data makes making
models extremely easy.

For example, we can create a `todo` model like so:

{% highlight javascript %}
// models/todo.js
import DS from 'ember-data';

const { Model, attr, hasMany } = DS;

export default Model.extend({
  title: attr('string'),
  isCompleted: attr('boolean'),
  quickNotes: hasMany('quick-note')
});

// models/quick-note.js
import DS from 'ember-data';

const { Model, attr, belongsTo } = DS;

export default Model.extend({
  name: attr('string'),
  todo: belongsTo('todo')
});

{% endhighlight %}

Note, that filenames should be all lowercase and dasherized - this is used by the
*Resolver* automatically.

### Adapters & Serializers

Ember Data makes heavy use of *per-type* adapters and serializers. These objects
can be resolved like any other.

Adapters can be placed at `/app/adapters/type.js`:

{% highlight javascript %}
// adapters/post.js
import DS from 'ember-data';

const { RESTAdapter } = DS;

export default RESTAdapter.extend({});
{% endhighlight %}

And its serializer can be placed in `/app/serializers/type.js`:

{% highlight javascript %}
// serializers/post.js
import DS from 'ember-data';

const { RESTSerializer } = DS;

export default RESTSerializer.extend({});
{% endhighlight %}

Application-level (default) adapters and serializers should be named
`adapters/application.js` and `serializers/application.js`, respectively.

### Mocks

Ember CLI comes with an **http-mock** generator which is preferred to
fixtures for development. Mocks have several advantages
over fixtures, a primary one being that they interact with your
application's adapters. Since you'll eventually be hooking your app
up to a live API, it's wise to be testing your adapters from the onset.

To create a mock for a `posts` API endpoint, use

{% highlight bash %}
ember g http-mock posts
{% endhighlight %}

A basic [ExpressJS](http://expressjs.com/) server will be scaffolded for
your endpoint under `/your-app/server/mocks/posts.js`. Once you add the
appropriate JSON response, you're ready to go. The next time you run
`ember server`, your new mock server will be listening for any API requests
from your Ember app.

> Note: Mocks are just for development. The entire `/server`
directory will be ignored during `ember build` and `ember test`.

If you decide to use fixtures instead of mocks, you'll need to use
`reopenClass` within your model class definitions. First, create a fixture
adapter, either for a single model or your entire application:

{% highlight javascript %}
// adapters/application.js
import DS from 'ember-data';

const { FixtureAdapter } = DS;

export default FixtureAdapter.extend({});
{% endhighlight %}

Then add fixture data to your model class:

{% highlight javascript %}
// models/author.js
import DS from 'ember-data';

const { Model, attr } = DS;

let Author = Model.extend({
  firstName: attr('string'),
  lastName: attr('string')
});

Author.reopenClass({
  FIXTURES: [
    {id: 1, firstName: 'Bugs', lastName: 'Bunny'},
    {id: 2, firstName: 'Wile E.', lastName: 'Coyote'}
  ]
});

export default Author;
{% endhighlight %}

Your Ember app's API requests will now use your fixture data.
