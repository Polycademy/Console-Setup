WiFi
====

Stats
-----

Use this command: `netsh wlan show networks mode=bssid`.

The above command will show signal strength, wifi channel, wifi protocol used, encryption, and data rates.

On Windows, quality is defined in terms of percentage instead of dBm. To do necessary conversion you can use this formula: http://stackoverflow.com/a/15798024/582917

Linux commands are found here: http://www.cyberciti.biz/tips/linux-find-out-wireless-network-speed-signal-strength.html

HotSpot
-------

If you're connected to the internet, you can use your WiFi as a hotspot access point, and share your internet connection for your other WiFi enabled devices. You can think of this as reverse tethering. The official name for this feature is: "Windows Hosted Wireless Network".

This can work by routing:

* USB Modem <-> WiFi Hotspot
* Ethernet <-> WiFi Hotspot
* WiFi <-> WiFi Hotspot

To do this, we need to enable the `hostednetwork` interface on our WiFi interface. This creates a virtual WiFi Hotspot interface. Then we start ICS (Internet Connection Sharing) on the interface that is connected to the internet, and link it up with the WiFi Hotspot interface.

First go into admin mode in Cygwin. Your WiFi must be switched on.

Only modern WiFi interfaces support the `hostednetwork` capability:

```
netsh wlan show hostednetwork
```

Create your `hostednetwork` where `*` can be anything (this command can be used to change the SSID and key of the access point):

```
netsh wlan set hostednetwork mode=allow ssid=* key=*
```

Start your `hostednetwork`:

```
netsh wlan start hostednetwork
```

By default the `hostednetwork` has a fixed IP address of `192.168.137.1` and fixed subnet mask of `255.255.255.0`. This is fixed by the WiFi interface. (It however can be changed via the registry: http://www.tomshardware.com/faq/id-1925829/change-default-internet-connection-sharing-address-range.html) This is will be the IP of your WiFi access point. Your mobile devices will acquire an IP of `192.168.137.*`.

For Windows XP see: http://www.utilizewindows.com/networking/implementation/406-internet-connection-sharing-ics-on-windows-systems

Now you need to setup ICS.

Connect your WiFi enabled device.

Run this command to see which devices are connected:

```
netsh wlan show hostednetwork
```

Stop your `hostednetwork` (by default on shutdown, the `hostednetwork` is stopped automatically):

```
netsh wlan stop hostednetwork
```

Disable your `hostednetwork`:

```
netsh wlan set hostednetwork mode=disallow
```

The problem with the DNS is because the WiFi HotSpot is not running its own DNS server. When peeps connect to 192.168.137.1, they expect the DNS server/GateWay IP to be that as well. It seems adding the DNS servers to that interface doesn't do anything, as that field specifies what DNS server for connections on 192.168.137.1 to be using, and in fact, the client connections don't actually use that DNS server, that's like the DNS servers that the WiFi hotspot interface would use if it were using the internet. The real DNS is in the TELKOMCEL, that is automatically acquired, and passed on from TELKOMCEL. The computer currently is using that internet interface, so it uses the DNS passed from TELKOMCEL. Now why has nobody mentioned this, and the ICS not passing the DNS servers to the WiFi hotspot interface and all client connections?