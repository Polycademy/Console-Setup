Local DNS
=========

Think of a local DNS as more powerful form of the `/etc/hosts` file. Rather than just statically specifying mappings between hostnames to IPs, you get access to caching, wildcards, programmability.. etc.

With local DNS, you often need the ability to create virtual network adapters/interfaces as well.

Otherwise you'll need to work out your own /etc/hosts automation. You can use these: https://github.com/smdahlen/vagrant-hostmanager

So there are 3 options:

1. Use local DNS and use 127.X.X.X mapping. And ipv6 mapping of course as well! Then you'll just need software defined localhost. This means you can bind services to 127.0.0.2 and use the same port as a service binding to 127.0.0.1.
2. Use local DNS along with a virtual network adapter/interface. This is a bit more complicated on Windows, as you'll need some sort of software to create these interfaces. Virtualbox can do it. You can also use the Microsft KM Test Loopback adapter. Note that the loopback adapter even though it might have IP like 192.168.X.X, it still just loopbacks to localhost.
3. Just use a etc/hosts automation tool with vagrant, so the the hostname to IP mappings are stored inside the Vagrantfiles.

In all of these solutions, there may be a need for a local DHCP server.

See this list for reserved IP addresses: http://en.wikipedia.org/wiki/Reserved_IP_addresses

DNS Software
------------

* http://passingcuriosity.com/2013/dnsmasq-dev-osx/
* Acrylic on Windows
* MaraDNS for Cygwin 32bit and linux
* en.wikipedia.org/wiki/Comparison_of_DNS_server_software

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

Now run `loopback-manager` as adminitrator.

You can now add virtual network adapters that are loopback adapters.

You need to restart for them to work.

Also they will not be assigned any IPs. To assign them IPs, you need can use:

* http://technet.microsoft.com/en-us/library/hh826150.aspx
* http://www.microsoft.com/resources/documentation/windows/xp/all/proddocs/en-us/netsh.mspx?mfr=true
* Or manually do it in your Network Connections