PAAS Tools
==========

Pagodabox Setup
---------------

First download the Pagodabox CLI tool: http://pagodabox.io/downloads/

Put it in `~/bin`.

Run `pagodabox`.

Put in your credentials.

Configuration will be stored in `~/.pagodabox`.

Heroku Setup
------------

First install Ruby and Ruby Gems.

Run these:

```sh
gem install rb-readline
```

Then download the Heroku client: https://github.com/heroku/heroku using the Tarball style.

Extract and add the Heroku script to your PATH.

```sh
ln -s /cygdrive/c/Users/CMCDragonkai/heroku-client/bin/heroku bin/heroku
```

Then login and enjoy:

```
heroku login
```