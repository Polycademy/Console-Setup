Python Setup
------------

Use pyenv. However right now, it needs some packages that might not be available on the 64bit version.

Issues
------

Waiting on 64bit packages for llvm. These are needed for making Python from source.

Temporary Solution
------------------

Get the normal python and python3 pip from Cygwin packages.

Note that there is bug with Cygwin 64bit: http://stackoverflow.com/questions/18947163/uuid-python-import-fails-on-cygwin-64bits