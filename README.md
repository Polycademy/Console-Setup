Console Setup
=============

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
* utils-linux
* cygutils
* diffutils

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

First disable it in Cygwin's mouse settings which can be accessed via right clik. Then disable in ConEmu's settings.

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
	SSHAGENT=/usr/bin/ssh-agent
	SSHAGENTARGS="-s"
	if [ -z "$SSH_AUTH_SOCK" -a -x "$SSHAGENT" ]; then
		eval `$SSHAGENT $SSHAGENTARGS` > /dev/null
		trap "kill $SSH_AGENT_PID" 0
	fi
    # Setup some aliases
    alias apt-cyg-main='apt-cyg -m http://mirrors.kernel.org/sourceware/cygwin'
    alias apt-cyg-port='apt-cyg -m ftp://ftp.cygwinports.org/pub/cygwinports'
# End of Custom Configuration
```

Setting up SSH
--------------

Create a `.ssh` folder at your home directory.

Follow the instructions here (https://help.github.com/articles/generating-ssh-keys) to create an `id_rsa` and `id_rsa.pub`.

Send the public key to wherever you want to access. It's public!

Once you've added your keys to your `ssh-agent`, you can show your currently added keys:

```
ssh-add -l
```

To remove the cached keys:

```
ssh-add -D
```

This means now for any git repositories, you must use the SSH clone URL, not the HTTPS url. Only the SSH clone URL works with SSH keys.
So if you're cloning projects, always the SSH clone URL. If you have projects that is already using the HTTPS url, you can convert them:

```
git remote set-url REMOTENAME SSHURL
```

For example:

```
git remote set-url origin git@github.com:Polycademy/Console-Setup.git
```

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

Setting Up SSH Server
---------------------

Use this: http://www.larsavery.com/blog/how-to-install-sshd-secure-shell-server-on-windows-using-cygwin/

Once this is go into `Start > Settings > Control Panel > Administrative Tools > Services` and look for `CYGWIN sshd`, and change the startup type to manual or automatic depending on whether you want sshd to be running on Windows bootup.

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
