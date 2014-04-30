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