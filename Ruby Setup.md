Ruby Setup
==========

RVM works perfectly with Cygwin 32bit or 64bit. Just make sure that the Cygwin setup exe is located in your PATH.

RVM will automatically attempt to install dependencies via Cygwin's setup unattended.

Then just run this:

```
curl -sSL https://get.rvm.io | bash -s stable --ruby
```

Note that RVM will autoadd entries into your `.zshrc`, `.bashrc` and `.bash_profile` and `.profile` and `.mkshrc`. You should remove these auto added entries.

These 2 entries are required for RVM and Ruby to work:

```
export PATH="$PATH:$HOME/.rvm/bin"
[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```

These lines will be autoadded. If something does not work, look for these lines.

Note that since these lines are autoadded, if you already have these preconfigured, go there and delete the newly added lines!

Just add this to `.zshrc`:

```
if [ -d "${HOME}/.rvm" ] ; then
    export PATH="${HOME}/.rvm/bin:${PATH}"
    source "$HOME/.rvm/scripts/rvm"
fi
```