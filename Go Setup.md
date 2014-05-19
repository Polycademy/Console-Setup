Go Setup
========

Get GVM. https://github.com/moovweb/gvm

Make sure to add this to your `.zshrc`:

```sh
    # Source GVM
    export GVM_ROOT=/cygdrive/c/Users/CMCDragonkai/.gvm
    . $GVM_ROOT/scripts/gvm-default
```

Issues
------

Currently waiting on a bugfix regarding the use of make.bash vs make.bat.

https://github.com/moovweb/gvm/issues/67

Temporary Solution
------------------

Just install official Windows version of Go.

Get the zip file. And unzip into one of a new bin directory at your home called ~/.go

Run this in cmd:

```
SETX GOROOT %HOME%/.go
```

This is required because you're using the Window's version of Golang, and it will use the Windows environment variables instead of Cygwin's environment variables. Therefore you need GOROOT to be set from CMD.

Add this to your zshrc:

```
# go (windows installation)
if [ -d ${HOME}/.go ] ; then
    export PATH="${GOROOT}/bin:${PATH}"
fi
```

Now we need something to manage GOPATH which is basically the path to your current Go workspace.