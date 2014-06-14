Fiddler2 Setup
==============

Fiddler 2 is pretty cool.

Setup Windows 8 as usual. (Remember since it's decrypting HTTPS traffic, you might to temporarily accept the Fiddler2 certificate when using things like Firefox... etc.)

Make sure the connection port is on something like 10000. 8888 is probably used by other things.

Install the Firefox extension, and make the connection to Fiddler2 automatic.

Requests through Cygwin will not be captured usually because it doesn't use Window's system proxy.

You'll need this inside your `.zshrc` in order for programs to automatically use the proxy. Some programs won't check these variables however you would need to set it up yourself:

```zsh
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
```

Running `set-system-proxy` should only be used when fiddler2 is running. When it is not running, make sure to unset it.