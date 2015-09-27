Oh-My-ZSH Setup
===============

Download and install my fork of ZSH. https://github.com/CMCDragonkai/oh-my-zsh

Make sure to clean up your `.zshrc` afterwards since oh-my-zsh will screw it up. But it will save a backup.

Then add this to the `.zshrc`:

```
# oh-my-zsh
if [ -d "${HOME}/.oh-my-zsh" ] ; then
    export ZSH=$HOME/.oh-my-zsh
    ZSH_THEME="dragon"
    CASE_SENSITIVE="true"
    # DISABLE_UNTRACKED_FILES_DIRTY="true" # uncomment for large git repositories
    HIST_STAMPS="dd-mm-yyyy"
    plugins=(git)
    . $ZSH/oh-my-zsh.sh
    unalias gm # because this is for graphics magick!
fi
```