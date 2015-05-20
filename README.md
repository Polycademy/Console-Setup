Console Setup
=============

TODO:

1. Convert all of these into a dot file and bash setup script. (http://dotfiles.github.io/)
2. Incorporate a ZSH package manager: https://github.com/Tarrasch/antigen-hs/blob/master/README.md
3. Start using the XDG standards such as the `~/.config` directory for all the config files.
4. Reinstall Ruby, Gems, RVM
5. Complete Compilation of FFMPEG
6. Download MINGW libraries, and compile to target MINGW software that is not compatible with Cygwin. (well anything that doesn't already have Windows binary version). Note that MINGW seems to compile and understand only Windows PATHs not Cygwin PATHs. (http://stackoverflow.com/questions/771756/what-is-the-difference-between-cygwin-and-mingw/792422?noredirect=1#comment45956881_792422)

Installations
-------------

* zsh
* openssh
* openssl
* wget
* curl
* git
* whois
* make
* vim
* nano
* psmisc
* procps
* ncurses
* dos2unix
* mosh
* mercurial
* binutils
* bison
* gcc (for C and C++) (get this from Cygwin setup not apt-cyg)
* autoconf
* automake
* libxml2
* libyaml
* weechat
* mkisofs
* gnupg
* GraphicsMagick
* inetutils
* inetutils-server
* nc
* socat
* pv
* utils-linux
* cygutils
* diffutils
* colorgcc
* patch
* cmake
* libboost
* libboost-devel
* cygutils-extra
* pkg-config

Setting Home to Windows User Profile
------------------------------------

1. In CMD:

```
SETX HOME %USERPROFILE%
```

2. Change the home directory in `/etc/passwd' so that any SSH or Telnet session will use the Windows User Profile as the home directory.

```
Username:...random stuff here...:/home/Username:/bin/zsh
--->
Username:...random stuff here...:/cygdrive/c/Users/Username:/bin/zsh
```

3. Copy over any `.rc` and `.profile` files from `/home/User` to `%USERPROFILE%`. This way you preserve your shell settings.

Setting up ZSH
--------------

Go into `/etc/passwd`, change the `:/bin/bash` to `:/bin/zsh` for the users that want ZSH as their default shell.

You can also edit the Cygwin.bat in the Cygwin installation directory. And change the `bash --login -i` to `zsh -l -i`

To allow Window's Paths
-----------------------

Run this in CMD as administrator:

```
SETX CYGWIN nodosfilewarning /m
```

This will set a global system environment variable. Cygwin will no longer complain about Window's style autocompletion of paths.

Installing apt-cyg
------------------

`apt-cyg` will allow you to install Cygwin packages from inside Cygwin.

This is the apt-cyg repository: https://github.com/transcode-open/apt-cyg

Install it by getting the apt-cyg binary and putting it into `/bin/apt-cyg`.

Then run:

```
chmod +x /bin/apt-cyg
```

Do note that when you install from cygwin-ports, the automated installer will attempt to look for dependencies. These dependencies may only exist in cygwin-main. This means often you'll need to backtrack, look for the missing dependencies and reinstall them from main first before then installing them from ports.

Apt-Cyg currently has a problem: https://github.com/transcode-open/apt-cyg/issues/37 (you've added branching logic to the apt-cyg file, it's just annoying now.)

Getting back the `clear` command
--------------------------------

`clear` can be reaquired by installing `ncurses`.

```
apt-cyg install ncurses
```

Combining ConEmu with Cygwin
----------------------------

Go into the settings of ConEmu, and create 2 tasks called Cygwin and Cygwin (Admin).

In Cygwin, the command is:

```
C:\cygwin64\bin\mintty.exe -i /Cygwin-Terminal.ico -
```

In Cygwin (Admin), the command is:

```
C:\cygwin64\bin\mintty.exe -new_console:a -i /Cygwin-Terminal.ico -
```

Then go into Startup, and select Cygwin as the `Specified named task` so that Cygwin becomes the default task for ConEmu.

Configuring ZSH for Fun & Profit
--------------------------------

http://zanshin.net/2013/02/02/zsh-configuration-from-the-ground-up/

Disabling Copy on Selection
---------------------------

First disable it in Cygwin's mouse settings which can be accessed via right click. Then disable in ConEmu's settings.

Getting sane copy & paste
-------------------------

On some keyboards insert doesn't exist. So you would want to get simple `Ctrl + Shift + C` or `Ctrl + Shift + V`.

To do so, in Cygwin's settings, go into the keys panel and switch on `Ctrl + Shift letter shortcuts`.

These become available:

```
Ctrl+Shift+C: Copy
Ctrl+Shift+V: Paste
Ctrl+Shift+N: New
Ctrl+Shift+Q: Close
Ctrl+Shift+R: Reset
Ctrl+Shift+D: Default size
Ctrl+Shift+F: Full screen
Ctrl+Shift+S: Flip screen
```

Understanding ZSH's Config File Loading Order
---------------------------------------------

See: http://shreevatsa.wordpress.com/2008/03/30/zshbash-startup-files-loading-order-bashrc-zshrc-etc/

In the case of ZSH, the `.zshrc` is always read whether it's an interactive or login shell.

In the case of BASH, the `.bash_profile` is read when it's a login shell, the `.bashrc` is read when it's an interactive shell.
Therefore put things into the `.bashrc` and source it from `.bash_profile`.

Example `.zshrc` file
---------------------

The bottom shows what you should probably have, and we have added a snippet to start ssh-agent upon startup.

```sh
# The following lines were added by compinstall
zstyle :compinstall filename '/cygdrive/c/Users/CMCDragonkai/.zshrc'

autoload -Uz compinit
compinit
# End of lines added by compinstall
# Lines configured by zsh-newuser-install
HISTFILE=~/.histfile
HISTSIZE=1000
SAVEHIST=1000
setopt appendhistory autocd extendedglob nomatch notify
unsetopt beep
bindkey -v
# End of lines configured by zsh-newuser-install
# Custom Configuration
    # Include user's private bin, make available to sub processes
    if [ -d "${HOME}/bin" ] ; then
      export PATH="${HOME}/bin:${PATH}"
    fi
    
    # Launch ssh-agent if it's not running
    # When the shell exits, kill the ssh-agent
    # Identities are remembered for 1 hour
    # Each new terminal gets its own ssh-agent
    SSHAGENT=/usr/bin/ssh-agent
    if [ -z "$SSH_AUTH_SOCK" -a -x "$SSHAGENT" ]; then
        eval `$SSHAGENT -s -t 3600` > /dev/null
        trap "kill $SSH_AGENT_PID" 0
    fi
    # Setup some aliases
    alias apt-cyg-main='apt-cyg -m http://mirrors.kernel.org/sourceware/cygwin'
    alias apt-cyg-port='apt-cyg -m ftp://ftp.cygwinports.org/pub/cygwinports'
# End of Custom Configuration
```

Setup Pkg-Config Path
---------------------

Add this:

```
export PKG_CONFIG_PATH="$PKG_CONFIG_PATH:/lib/pkgconfig:/usr/lib/pkgconfig:/usr/local/lib/pkgconfig"
```

It's required for source compilations for some software.

Notes regarding SourceTree
--------------------------

You want to use SSH for SourceTree instead of HTTPS.

So when installing SourceTree, make sure to select OpenSSH as your SSH client, not Putty.

SourceTree's OpenSSH doesn't support passwords, so you need to use SSH keys.

We have 3 things to do to make SourceTree work with SSH keys:

1. Go into Tools -> Options and in the SSH Client Configuration -> SSH Key input field, put the full path to `id_rsa` key. This means you're using the same `id_rsa` for any remotes that SourceTree is managing. Which probably means both Github and Bitbucket.

2. Go into Hosted Repositories and click on Edit Accounts, and change the Preferred Protocol to SSH.

3. Make sure to always clone from SSH URLs, not HTTPS URLs.

4. Run this as administrator from Cygwin/ConEmu, change the drive path to where SourceTree is installed. This is because you're using `ssh` from Cygwin a unix program, which is running `openssh_wrapper.sh` which is file written on Windows with `crlf` line endings.

```
dos2unix /cygdrive/c/Program\ Files\ \(x86\)/Atlassian/SourceTree/tools/openssh_wrapper.sh
```

Setting up Git
--------------

Make sure the `core.autocrlf` is false, and the default line ending is lf:

```
git config --global core.autocrlf false
git config --global core.eol lf
```

Cygwin's git-core templates is stored in a different place than usual. Run this at the command line:

```
git config --global init.templatedir /cygdrive/c/cygwin64/usr/share/git-core/templates
```

Change the path to wherever the actual `git-core/templates` are.

This one would solve the problem of SourceTree complaining about the git templates being missing.

Also the templates folder is completely editable, edit it to your heart's content. This way any new git repositories (inited or cloned) will have these templates inside their `.git` folder.

Other Cygwin Programs
---------------------

http://sourceware.org/cygwinports/

By using the aliases:

```
apt-cyg-main
apt-cyg-port
```

Cygwin 64bit vs 32bit
---------------------

They are currently incompatible. Some packages have not been ported to 64bit. This can be problematic.
Recommend installing 32bit for now if you're doing something you haven't done before. Otherwise check the 64bit packages first.

Making Cygwin Setup Executable from Commandline
-----------------------------------------------

Move the `setup-x86_64.exe` to the `~/bin`. This will allow certain tools that expect to use Cygwin's setup automatically. Such as for example the RVM for Windows.

Cool Unix Tools
---------------

http://kkovacs.eu/cool-but-obscure-unix-tools

Adding Fake Sudo
----------------

Some scripts will use `sudo`. Now `sudo` doesn't exist inside Cygwin. So we're going to create a fake sudo.

In your `~/bin`, add a `sudo` file and add this:

```
#!/bin/bash

"$@"
```

And add this alias to your `.zshrc`:

```
alias sudo='sudo '                  # allows sudo commands to use aliases
```

Making Sublime Text Editor executable from CLI
----------------------------------------------

```
cd ~/bin
ln -s "path/to/subl.exe" sublime
```

Note that this might be changed with the subl executable.

Making System Binaries Available on PATH
----------------------------------------

Cygwin doesn't add your system binaries to PATH. To fix this, add this to your `.zshrc`:

```
# Adding sbin to PATH
export PATH="$PATH:/sbin:/usr/sbin:/usr/local/sbin"
```

Setting up ngrok
----------------

Add this to your `.zshrc` after downloading and extracting ngrok into your PATH.

```
# ngrok, see help by doing "\ngrok --help", the "\" will run ngrok directly
hash ngrok 2>/dev/null && {
    start_ngrok () {
        cygstart powershell "ngrok $@"
    }
    alias ngrok=start_ngrok
}
```

256 Colors
----------

Right click on the terminal while you're inside mintty. Go into `Options -> Terminal -> Type`. Check whether the type is `xterm` or `xterm-256color`. If it's either, it's fine.

Then add this to your `.zshrc`:

```
export TERM=xterm-256color
```

Getting Clipboard Functionality
-------------------------------

Allowing you to use `putclip` and `getclip` for copying data from commands, along with some other interesting utilities.

```
apt-cyg-main install cygutils-extra
```

Checking all Installed Packages
-------------------------------

```
apt-cyg list
```

Changing Maximum Memory of Cygwin Programs
------------------------------------------

https://cygwin.com/cygwin-ug-net/setup-maxmem.html

I think this only applies to executables compiled for Cygwin, not MINGW.