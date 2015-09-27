iperf Setup
===========

Get the windows version. Put it in Source. Add a symlink.

Test bandwidth:

```
# on server
iperf -s
# on client
iperf -c <ip of server>
```

iperf3 is not compatible with iperf2. So you could have 2 iperf, one for `iperf2` and another for `iperf3`.