Nmap & Ncat Setup
=================

Setting up nmap and ncat.

Acquire the command line zip file: http://nmap.org/download.html#windows

Install vcredist x64 (which includes x86) from the official Microsoft sources.

Install WinPcap from the zip file.

Do not make it start on boot.

Learn these commands (windows CLI commands are accessible from inside Cygwin):

```
# Start winpcap
net start npf
# Stop winpcap
net stop npf
# Change npf service to run manually
sc config npf start= demand
# Change npf service to run on boot
sc config npf start= auto
```

It's not a big deal whether npf starts on boot most of times. But I like to be in control of what is running on my computer, and not have things running uselessly.

Then extract the zip file, move it to a location for your utilities. Then:

```
ln -s "C:/absolute/path/to/nmap.exe" ~/bin/nmap
ln -s "C:/absolute/path/to/ncat.exe" ~/bin/ncat
ln -s "C:/absolute/path/to/nping.exe" ~/bin/nping
```

Use nping to test any kind of ports whether it's UDP, TCP or ICMP.