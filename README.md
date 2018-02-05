## EmberCLI's official website <a href="https://ember-community-slackin.herokuapp.com" target="_blank"><img src="https://ember-community-slackin.herokuapp.com/badge.svg"></a>

### Running with Docker (recommended)

This is the recommended method for new contributors.
Although the website is built with Ruby, most work is done in Markdown files.
You don't need to know Ruby or install its dependencies to help out. Simply follow
the Docker container instructions below to install and run locally.

First, install [Docker and Compose](https://store.docker.com/search?offering=community&type=edition) and leave it running.

Next, the commands below will install all necessary dependencies for the website
app and start a server. This will take a little while to run the first time,
possibly a few minutes. The dependencies will be installed inside a Docker
container, and do not affect your normal developer environment.

```bash
git clone https://github.com/ember-cli/ember-cli.github.io.git
cd ember-cli.github.io
docker-compose up
```

### Running without Docker

Install Jekyll and a special gem provided by GitHub:

```sh
$ gem install bundler
$ bundle
```

Open the directory which contains the repo and run Jekyll:

```sh
$ bundle exec jekyll serve -w
```

You can now view the result at [http://localhost:4000](http://localhost:4000).

## Windows Users, read on

If you want to help with the development of this site and you're using Windows,
please read [this guide](http://jekyll-windows.juthilo.com) about how to run
Jekyll on your OS.

Alternatively, use bash via Windows Subsystem for Linux (WSL):

- Install/setup [WSL][wsl-install]
- Install ruby, perhaps use [rbenv] or [linuxbrew.sh]

[wsl-install]: https://msdn.microsoft.com/en-us/commandline/wsl/install-win10
[rbenv]: https://github.com/rbenv/rbenv#installation
[linuxbrew.sh]: http://linuxbrew.sh
