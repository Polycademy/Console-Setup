ZSH Settings
============

Current settings:

```
# The following lines were added by compinstall
zstyle :compinstall filename '/cygdrive/c/Users/CMCDragonkai/.zshrc'

autoload -Uz compinit
compinit
# End of lines added by compinstall
# Lines configured by zsh-newuser-install
HISTFILE=~/.histfile
HISTSIZE=1000
SAVEHIST=1000
setopt appendhistory autocd extendedglob nomatch notify interactivecomments
unsetopt beep
bindkey -v
# End of lines configured by zsh-newuser-install

# ZSH Environment Variables

# Set MANPATH so it includes users' private man if it exists
if [ -d "${HOME}/man" ]; then
  export MANPATH="${HOME}/man:${MANPATH}"
fi

# Set INFOPATH so it includes users' private info if it exists
if [ -d "${HOME}/info" ]; then
  export INFOPATH="${HOME}/info:${INFOPATH}"
fi

# Include user's private bin, make available to sub processes
if [ -d "${HOME}/bin" ]; then
  export PATH="${HOME}/bin:${PATH}"
fi

# What's your editor?
export EDITOR="nano"

# What's your browser (also make this command executable in your PATH by using symlinks)
export BROWSER="firefox"

# 256 colors
export TERM=xterm-256color

# What's your language?
export LANG=en_GB.UTF-8

# Set the Windows Codepage to UTF8
chcp 65001 > /dev/null

# Adding sbin to PATH
export PATH="$PATH:/sbin:/usr/sbin:/usr/local/sbin"

# adding pkg-config 
export PKG_CONFIG_PATH="$PKG_CONFIG_PATH:/lib/pkgconfig:/usr/lib/pkgconfig:/usr/local/lib/pkgconfig"

# dig & host (bind tools)
if [ -d "${HOME}/.bind" ] ; then
    export PATH="${HOME}/.bind:${PATH}"
fi

# composer
export COMPOSER_HOME="$HOME/.composer"
if [ -d "${HOME}/.composer/vendor/bin" ] ; then
    export PATH="${HOME}/.composer/vendor/bin:${PATH}"
fi

# pyenv
if [ -d "${HOME}/.pyenv" ] ; then
    export PYENV_ROOT="${HOME}/.pyenv"
    export PATH="${PYENV_ROOT}/bin:${PATH}"
    eval "$(pyenv init -)"
fi

# gvm
if [ -d "${HOME}/.gvm" ] ; then
    export GVM_ROOT="${HOME}/.gvm"
    . $GVM_ROOT/scripts/gvm-default
fi

# go (windows installation)
if [ -d ${HOME}/.go ] ; then
    export PATH="${GOROOT}/bin:${PATH}"
fi

# rvm
if [ -d "${HOME}/.rvm" ] ; then
    export PATH="${HOME}/.rvm/bin:${PATH}"
    source "$HOME/.rvm/scripts/rvm"
fi

# phpbrew
if [ -d "${HOME}/.phpbrew" ] ; then
     . $HOME/.phpbrew/bashrc
fi

# nvm
if [ -d "${HOME}/.nvm" ] ; then
    . $HOME/.nvm/nvm.sh
fi

# node (windows installation)
if [ -d "${HOME}/.node" ] ; then
    export PATH="$HOME/.node:$PATH"
fi

# erlang (windows installation)
if [ -d "${HOME}/erlang" ] ; then
    export PATH="$HOME/erlang/bin:$PATH"
fi

# elixir (precompiled)
if [ -d "${HOME}/.elixir" ] ; then
    export PATH="$HOME/.elixir/bin:$PATH"
    # we have to use the windows versions
    alias elixir='elixir.bat'
    alias elixirc='elixirc.bat'
    alias iex='iex.bat'
    alias mix='mix.bat'
fi

# haskell platform
if [ -d "${HOME}/Haskell Platform" ] ; then
    export PATH="$HOME/Haskell Platform/bin:$PATH"
    export PATH="$HOME/Haskell Platform/lib/extralibs/bin:$PATH"
    export PATH="$HOME/Haskell Platform/winghci:$PATH"
    export PATH="$APPDATA/cabal/bin:$PATH"
    # cabal requires mingw gcc in order to compile certain packages
    run_cabal () {
        oldpath=$PATH
        export PATH="$HOME/Haskell Platform/mingw/bin:$PATH"
        cabal $@
        export PATH=$oldpath
    }
    alias cabal=run_cabal
fi

# rust (windows installation)
if [ -d "${HOME}/Rust" ] ; then
    export PATH="$HOME/Rust/bin:$PATH"
fi

# java (windows installation)
if [ -d "${HOME}/.jdk" ] ; then
    export JAVA_HOME="$HOME/.jdk"
    export PATH="$HOME/.jdk/bin:$PATH"
fi

# mecury (mingw installation)
if [ -d "/cygdrive/c/mercury-14.01-mingw/bin" ] ; then
    export PATH="/cygdrive/c/mercury-14.01-mingw/bin:$PATH"
fi

# ansible
hash ansible 2>/dev/null && {
    # Cygwin's openssh does support ControlMaster
    export ANSIBLE_SSH_ARGS="-o ControlMaster=no"
    export ANSIBLE_HOSTS="${HOME}/ansible_hosts"
}

# oh-my-zsh
if [ -d "${HOME}/.oh-my-zsh" ] ; then
    export ZSH=$HOME/.oh-my-zsh
    ZSH_THEME="dragon"
    CASE_SENSITIVE="true"
    # DISABLE_UNTRACKED_FILES_DIRTY="true" # uncomment for large git repositories
    HIST_STAMPS="dd-mm-yyyy"
    plugins=(git)
    . $ZSH/oh-my-zsh.sh
fi

# direnv
hash direnv 2>/dev/null && {
    eval "$(direnv hook zsh)"
}

# ngrok, see help by doing "\ngrok --help", the \ will run ngrok directly
hash ngrok 2>/dev/null && {
    start_ngrok () {
        cygstart powershell "ngrok $@"
    }
    alias ngrok=start_ngrok
}

# fiddler2
set-system-proxy () {
    local proxies=`powershell "(get-itemproperty 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings').ProxyServer"`
    if [ -n "$proxies" ]; then
        echo 'Setting Proxies:'
        proxies=("${(s/;/)proxies}")
        for proxy in $proxies; do
            echo $proxy
            local address=`echo $proxy | sed 's/.*=\(.*\)/\1/'`
            if [[ $proxy == 'http='* ]]; then
                export http_proxy="http://$address"
                export HTTP_PROXY="http://$address"
            elif [[ $proxy == 'https='* ]]; then
                export https_proxy="https://$address"
                export HTTPS_PROXY="https://$address"
            elif [[ $proxy == 'ftp='* ]]; then
                export ftp_proxy="ftp://$address"
                export FTP_PROXY="ftp://$address"
            fi
        done
    else 
        echo 'No system proxies found'
    fi
}

unset-system-proxy () {
    echo 'Unsetting Proxies'
    unset http_proxy
    unset HTTP_PROXY
    unset https_proxy
    unset HTTPS_PROXY
    unset ftp_proxy
    unset FTP_PROXY
}

# Launch ssh-agent if it's not running
# When the shell exits, kill the ssh-agent
# Identities are remembered for 1 hour
# Each new terminal gets its own ssh-agent
SSHAGENT=/usr/bin/ssh-agent
if [ -z "$SSH_AUTH_SOCK" -a -x "$SSHAGENT" ]; then
    eval `$SSHAGENT -s -t 3600` > /dev/null
    trap "kill $SSH_AGENT_PID" 0
fi

# Opinionated commands, you can disable the alias by prefixing commands with a \
apt-cyg-main () {
    apt-cyg mirror http://mirrors.kernel.org/sourceware/cygwin && apt-cyg $@
}
apt-cyg-port () {
    apt-cyg mirror ftp://ftp.cygwinports.org/pub/cygwinports && apt-cyg $@
}
alias parallel='parallel --no-notice' # remove the citing notice
alias cp='cp -i'                    # prompt for overwrite
alias mv='mv -i'                    # prompt for overwrite
alias df='df -h'                    # human readable
alias du='du -h'                    # human readable
alias grep="grep $GREP_OPTIONS"     # show differences in colour
alias egrep="grep -E $GREP_OPTIONS"    # show differences in colour
alias fgrep="grep -F $GREP_OPTIONS"    # show differences in colour
alias ls='ls -hF --color=tty'       # colour, readable sizes, indicator
alias ll='ls -hlF --color=tty'      # long format to show sizes
alias la='ls -AlF'                  # all but . and ..
alias sudo='sudo '                  # allows sudo commands to use aliases

# Deprecated GREP_OPTIONS
unset GREP_OPTIONS
```

Look into this: https://gist.github.com/agnoster/3712874