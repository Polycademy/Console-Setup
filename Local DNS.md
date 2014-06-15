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