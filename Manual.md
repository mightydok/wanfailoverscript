# Design #

## Assumption ##

I asume that your host has access to two different routers that act as the primary and backup gateway for the primary WAN connection and backup connection. It may be that your host has two interfaces, with two IP-addresses for each WAN connection, but this is not required.

## Link availability test ##

WFS tests for the availability of the primary WAN link by sending ICMP (PING) messages to user-specified target hosts. Two target hosts must be configured. If a host goes down, while our WAN connection is just perfectly fine, we don't want a failover. Thus, as a sanity check, a second host must be tested.

## Failover ##

WFS changes the default gateway and thus the default route to which all traffic is sent. It uses the native 'route' command found on most Linux distros.

## Restore primary WAN link ##

When executed, WFS adds static routes for the two target hosts that are used for availability monitoring. These routes specifically use the router/gateway for the primary WAN interface, even if the default gateway is switched to the failover connection.

This allows WFS to monitor if the primary WAN interface is still down or back up again. If the latter is true, then WFS will switch the default route back to the primary interface. Example:

The primary WAN gateway is 10.0.0.1 in this example.

```
server:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
74.125.77.104   10.0.0.1        255.255.255.255 UGH   0      0        0 eth1
209.85.229.104  10.0.0.1        255.255.255.255 UGH   0      0        0 eth1
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0
10.0.0.0        0.0.0.0         255.255.255.0   U     0      0        0 eth1
0.0.0.0         10.0.0.1        0.0.0.0         UG    0      0        0 eth1
```

When a failover to a backup WAN link occurs (192.168.0.1) it would look like this:

```
server:~# route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
74.125.77.104   10.0.0.1        255.255.255.255 UGH   0      0        0 eth1
209.85.229.104  10.0.0.1        255.255.255.255 UGH   0      0        0 eth1
192.168.0.0     0.0.0.0         255.255.255.0   U     0      0        0 eth0
10.0.0.0        0.0.0.0         255.255.255.0   U     0      0        0 eth1
0.0.0.0         192.168.0.1     0.0.0.0         UG    0      0        0 eth0
```


## Configuration options ##

To configure WFS to your needs, edit the WFS script itself or edit /etc/wfs.conf.

```
PRIMARY_TARGET=74.125.77.104
SECONDARY_TARGET=209.85.229.104 
```

These are the hosts that are used to test if the primary WAN interface is available. Please note that this is an example. These hosts are IP-addresses used by Google.

```
PRIMARY_GW=10.0.0.1
SECONDARY_GW=192.168.0.1
```

Here, the primary and secondary (failover)  WAN gateway adress is configured. When a failure is detected, WFS will switch from the primary gateway to the secondary gateway.

```
INTERVAL=30
COUNT=3
```

WFS will tests the availability of the primary WAN link each 'INTERVAL' seconds, in this example thus 30 seconds. WFS will tests by sending 3 ICMP messages.

```
THRESHOLD=2
```

To prevent a premature failover, such as when connectivity is only lost for a couple of seconds, a threshold must be reached before a failover is performed. In this example, the failover test must fail twice before a failover is committed. Based on the interval used in the previous example, a failover will take about a minute after the connection has gone bad. By changing the values, you can switch faster or later.

```
COOLDOWNDELAY=120
```

To prevent fast switching between primary and secondary WAN link due to a flaky connection, a cool down period is used. Only after this period WFS will switch back to the primary interface again.

```
MAIL_TARGET=""
```

Assuming that your backup WAN link allows you to send email messages, you can specify an email address. This address will receive a message when a failover occurs or when the primary link is restored.

```
PRIMARY_CMD=""
SECONDARY_CMD=""
```

These two variables can be used to execute a custom command after a failover or restore is performed.