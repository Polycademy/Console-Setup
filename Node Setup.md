Node Setup
==========

Use NVM then add this to your `.zshrc`:

```
if [ -d "${HOME}/.nvm" ] ; then
    . $HOME/.nvm/nvm.sh
fi
```

However it's not currently working on Windows, because Cygwin cannot build Node.js

Workaround
----------

Use the official Windows manual installation (put it all in `~/.node`):

https://github.com/joyent/node/wiki/installation#manual-install

Then add these to `.zshrc`.

```
# node (windows installation)
if [ -d "${HOME}/.node" ] ; then
    export PATH="$HOME/.node:$PATH"
fi
```

Create a file called npm with

```
#!/bin/bash

DIR="$( cd "$( dirname "${BASH_SOURCE[0]}" )" && pwd )"

$DIR/npm.cmd "$@"
```

Then chmod 777.

```
chmod 777 npm
```

This will allow you to execute `npm` without `npm.cmd`.

Now try this:

```
npm install -g bower
```

It should be installed into your `.node` directory.

Problems
--------

Note that the normal `node` console cannot be launched from inside mintty which is the default terminal used by Cygwin. You can however launch it from Cygwin.bat or Powershell or CMD. 

If you feel that you get weird errors, just run: `cygstart /bin/zsh` to get non-mintty terminal, and then run your commands from there.