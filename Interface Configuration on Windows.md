Interface Configuration for Windows
===================================

Powershell commands:

```
Get-NetAdapter
$adapter = Get-NetAdapter -Name WiFi
Get-NetIPAddress -InterfaceAlias $adapter.Name
Get-NetIPConfiguration -InterfaceAlias $adapter.Name

# this adds a new ip address to an interface
New-NetIPAddress –InterfaceAlias "WiFi" –IPv4Address "192.168.0.1" –PrefixLength 24 -DefaultGateway 192.168.0.254

# this seems to do it as well, I'm not sure of the difference between New and Set
Set-NetIPAddress –InterfaceAlias "WiFi" –IPv4Address "192.168.0.1" –PrefixLength 24 -DefaultGateway 192.168.0.254
```

See this: http://blogs.technet.com/b/askpfeplat/archive/2013/03/29/mailbag-how-do-you-set-network-adapter-settings-with-powershell-in-windows-8-or-windows-server-2012.aspx