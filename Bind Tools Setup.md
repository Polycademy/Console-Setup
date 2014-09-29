Bind Tools Setup
================

Download Bind tools for Windows here: https://www.isc.org/downloads/

Extract the Bind Tools.

Create a `.bind` directory: `mkdir ~/.bind`.

Copy over these files over to the `~/.bind` directory:

```
dig.exe
host.exe
libbind9.dll
libdns.dll
libeay32.dll
libisc.dll
libisccfg.dll
liblwres.dll
libxml2.dll
```

Add the `~/.bind` directory to your PATH.

```
# dig & host (bind tools)
if [ -d "${HOME}/.bind" ] ; then
    export PATH="${HOME}/.bind:${PATH}"
fi
```

Install `vcredist_xXX.exe` from the extracted Bind folder, where XX is either 86 or 64.

Resolve
-------

You can add a `resolv.conf` by following these steps:

* http://thelowedown.wordpress.com/2010/02/03/dns-dig-on-windows/
* http://en.wikipedia.org/wiki/Resolv.conf

The `resolv.conf` will be automatically inferred from registry if it doesn't exist.