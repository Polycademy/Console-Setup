Ping
====

By default Windows prevents inbound ping from public/private/domain networks. This is pretty annoying if you're doing some virtualised sysadmin work.

To enable both ipv4 and ipv6 ping, run this inside powershell (admin mode).

```
Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv4-In)" -enabled True
Set-NetFirewallRule -DisplayName "File and Printer Sharing (Echo Request - ICMPv6-In)" -enabled True
```

You should disable pings in hostile networks.

Those names are relevant to Windows 8 Surface Pro and probably some other Windows versions as well. Microsoft sometimes renames the ping rules. In case you can't use them, you can try finding the real rule names, or creating your own ping rules.