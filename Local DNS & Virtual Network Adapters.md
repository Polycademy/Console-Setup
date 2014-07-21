Local DNS & Virtual Network Adapters
====================================

Think of a local DNS as more powerful form of the `/etc/hosts` file. Rather than just statically specifying mappings between hostnames to IPs, you get access to caching, wildcards, programmability.. etc.

With local DNS, you often need the ability to create virtual network adapters/interfaces as well.

Otherwise you'll need to work out your own /etc/hosts automation. You can use these: https://github.com/smdahlen/vagrant-hostmanager

So there are 3 options:

1. Use local DNS and use 127.X.X.X mapping. And ipv6 mapping of course as well! Then you'll just need software defined localhost. This means you can bind services to 127.0.0.2 and use the same port as a service binding to 127.0.0.1.
2. Use local DNS along with a virtual network adapter/interface. This is a bit more complicated on Windows, as you'll need a virtual network adapter manager to create interfaces. Many Virtualisation tools can do this, but the official Windows way is through `devcon.exe` and the Microsoft KM Test Loopback Adapter. Note that the loopback adapter even though it might have IP like 192.168.X.X, it still just loopbacks to localhost.
3. Just use a etc/hosts automation tool with vagrant, so the the hostname to IP mappings are stored inside the Vagrantfiles.

See this list for reserved IP addresses: http://en.wikipedia.org/wiki/Reserved_IP_addresses

DNS Software
------------

For Windows, we're going to use the Acrylic DNS Server.

Download the Acrylic DNS software from: http://mayakron.altervista.org/wikibase/show.php?id=AcrylicHome

This will install Acrylic into your Program Files and also a background service which will be launched and running via windows services. It is configured to run automatically on startup.

We're going to link its binary into our PATH so we can use it from Cygwin.

```
ln -s "/cygdrive/c/Program Files (x86)/Acrylic DNS Proxy/AcrylicController.exe" ~/bin/
```

There's another tool called `AcrylicConsole.exe`, but that is only used for debugging, or when you want to run the service on the foreground.

With this executable in our PATH, we can now execute it by typing `AcrylicController`. This executable does not have a help file but here are the commands:

```
AcrylicController StopAcrylicService
AcrylicController StartAcrylicService
AcrylicController PurgeAcrylicCache
AcrylicController EnableDisableAcrylicDebugLog
AcrylicController BrowseAcrylicDebugLog
AcrylicController EditAcrylicHostsFile
AcrylicController EditAcrylicConfigurationFile
```

The last 2 commands in editing the hosts file and configuration files currently do not work in Cygwin. You can edit them at their installation directory however:

```
sublime C:\Program Files (x86)\Acrylic DNS Proxy\AcrylicConfiguration.ini
sublime C:\Program Files (x86)\Acrylic DNS Proxy\AcrylicHosts.txt
```

Before we go ahead to configure it, let's use the namebench tool to find our nearest DNS servers. You should run this tool whenever you change geographic locations. For me, the namebench tool has determined that these are the best DNS servers to use:

```
203.134.64.66 iPrimus Sydney AU
8.8.8.8       Google Public DNS
211.26.25.90  iPrimus Perth-2 AU
```

Our local DNS server will act both as a recursive DNS server and authoritative DNS server. We want it to be recursive so it will forward it to the public DNS servers. We want it to be authoritative for development, so we can point it to our local web applications.

Ok time to configure the `AcrylicConfiguration.ini` and `AcrylicHosts.txt`:

To edit these files, you must run a text editing app at administrator level permissions.

Set the PrimaryServerAddress, SecondaryServerAddress, TertiaryServerAddress, they will be contacted in parrallel.

Reduce the cache time to:

```
AddressCacheNegativeTime=10
AddressCacheScavengingTime=600
AddressCacheSilentUpdateTime=450
```

This low DNS cache time is used so that we can get fast updates to whenever we're changing our DNS settings. If we're changing our DNS settings a lot, we might as well disable the cache too!

Now edit the `AcrylicHosts.txt`, this is just like the normal Windows hosts file, but it's more flexible in that it allows you to setup wildcards and regular expressions.

I set up an extra record like:

```
127.0.0.1 *.dev
```

This makes sure that any addresses with the `.dev` top level domain will route to localhost. To prove it try this:

```
nslookup random.dev
nslookup crazy.dev
```

All of it will use your local DNS server and resolve to 127.0.0.1. Note that your Windows hosts file will still be used, so any records there will be automatically resolved as well as the records in your `AcrylicHosts.txt`.

If you have a virtual network adapter setup, you can also point your `*.dev` hostnames to the IP address range on your virtual network adapter. However this means you'll need to setup an IP address for each hostname. This is because virtual machines will establish a bridge on your virtual network adapter and be assigned a static IP in the adapter's range. Which is bit more complex, and for that, you might as well just use the normal `etc/hosts`.

Once you have done all the configuration. Just restart the service, and then swap over your main network adapters (ethernet + wifi) to use the local 127.0.0.1 DNS service. The AcrylicService listens on UDP port 53 on 127.0.0.1. It does not listen on IPv6 [::1]. This is why you don't assign the IPv6 DNS entries on your networks. You can tell by running `netstat -ab`, or opening Windows Resource Monitor -> Networks.

Once this is all working, this allows you to use your official website hostnames to contact your local webservers whether on your host machine or guest virtual machine inside Virtualbox/Vagrant combo.

You'll also enjoy faster DNS, since you're using simultaneous DNS servers to acquire the fastest results, which is also a form of load balancing incase any one of those servers failed. Your local DNS server will probably never fail, and you have local caching too!

If websites don't work, you should check if the DNS service is running or not! It should on startup now.

Also you should start using Acrylic's hosts file instead of your normal hosts file.

Virtual Network Adapters
------------------------

First we need the `devcon.exe`. To install this we need Windows Visual Studio, Windows WDK and Windows SDK. The instructions are listed here: http://msdn.microsoft.com/en-us/library/windows/hardware/ff544707%28v=vs.85%29.aspx and http://plugable.com/2014/01/21/installing-devcon-on-windows-8-1

Once it is all installed we need to get the `devcon.exe` onto our PATH.

Run this (assuming 64bit and Windows 8.1 version):

```
ln -s "/cygdrive/c/Program Files (x86)/Windows Kits/8.1/Tools/x64/devcon.exe" ~/bin/devcon.exe
```

Then add this script to your `~/bin/loopback-manager` (chmodded to 777):

```bash
#!/usr/bin/env bash

{

    ident () {

        local s=$(printf '%*s' $1 "")
        shift
        printf "$s%s\n" "${@//$'\n'/$'\n'$s}"

    }

    calc () {

        awk "BEGIN {
            print $*
        }"

    }

    esc="\x1b["
    creset=$esc"39;49;00m"
    white=$esc"01;37m"
    green=$esc"32;01m"
    yellow=$esc"33;01m"
    magenta=$esc"35;01m"

    echo -e "$(tput bold)$(tput setaf 2)Windows Loopback Interfaces Manager$(tput sgr0)
${magenta}You must run this as administrator!${creset}

    ${green}1${white} List all installed Loopback interfaces
    ${green}2${white} Install a new Loopback interface (Reboot required)
    ${green}3${white} Remove a Loopback interface
    ${green}4${white} Remove all Loopback interfaces
    ${green}5${white} Reboot PC
${creset}"

    read -p "$(tput setaf 2)Enter an Option: $(tput sgr0)" -r option

    case $option in
        1)
            echo ""
            devcon find "*MSLOOP"
            echo ""
        ;;
        2)
            echo ""
            devcon install ${WINDIR}/inf/netloop.inf "*MSLOOP"
            echo "You must restart your computer!"
            echo ""
        ;;
        3)
            echo ""
            readarray -t interfaces <<< "`devcon find "*MSLOOP" | head -n -1 | awk '{print $1}'`"
            echo "$(tput setaf 2)Available interfaces:$(tput sgr0)"
            interface_choices=`printf "%s\n" "${interfaces[@]}"`
            echo ""
            
            index=0
            for interface_id in "${interfaces[@]}"; do
                echo "    $(tput setaf 2)${index}$(tput setaf 6) ${interface_id}$(tput sgr0)"
                index=`calc $index+1`
            done
            echo -e "${creset}"
            read -p "$(tput setaf 2)Choose one of the interfaces to delete: $(tput sgr0)" -r interface_to_delete

            if [[ ${interfaces[$interface_to_delete]} ]]; then
                echo "Deleting @${interfaces[$interface_to_delete]}"
                devcon remove "@${interfaces[$interface_to_delete]}"
            else
                echo "Incorrect key given."
            fi
        ;;
        4)
            echo ""
            devcon remove "*MSLOOP"
            echo ""
        ;;
        5)
            echo ""
            echo "Restarting computer!"
            shutdown /r /t 0
            echo ""
        ;;
    esac

}
```

You can now add virtual network adapters that are loopback adapters.

You need to restart for them to work.

Also they will not be assigned any IPs. To assign them IPs, you need can use:

* http://technet.microsoft.com/en-us/library/hh826150.aspx
* http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/netsh.mspx?mfr=true
* Or manually do it in your Network Connections

Note that this may be required:

http://louisgale.blogspot.com.au/2010/03/windows-virtual-pc-how-to-network.html

Once you created a network adapter, make sure to right click on its properties to set its drivers and its particular IP.