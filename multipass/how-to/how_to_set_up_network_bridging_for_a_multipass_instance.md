> See also: [Driver](/t/28410), [`local.driver`](/t/27357) 

This document demonstrates how to set up network bridging for a Multipass instance.

## Use VirtualBox to set up network bridging for a Multipass instance 

[tabs]

[tab version="Linux"]

This option only applies to macOS systems.

[/tab]

[tab version="macOS"]

Network bridging lets you add a second network interface to the instance and expose it on your physical network. 

First, stop the instance:

```plain
multipass stop primary
```

Now, find the network interface you want to bridge with, running the command: 

```plain
VBoxManage list bridgedifs | grep ^Name:
```

You want to find the identifier before the second colon; for example `en0` in the following sample output:

```plain
Name:            en0: Ethernet
Name:            en1: Wi-Fi (AirPort)
Name:            en2: Thunderbolt 1
Name:            en3: Thunderbolt 2
...
```

Finally, tell VirtualBox to use it as the "parent" for the second interface (see more info on bridging in [VirtualBox documentation](https://www.virtualbox.org/manual/ch06.html#network_bridged) on this topic):

```plain
sudo VBoxManage modifyvm primary --nic2 bridged --bridgeadapter2 en0
```

[note=caution]
Do not touch --nic1 as that's used by Multipass.
[/note]

You can then start the instance again:

```plain
multipass start primary
```

To find the name of the new interface, run this command:

```plain
multipass exec primary ip link | grep DOWN
```

In the following sample output, the name of the interface that we are looking for is `enp0s8`:

```plain
3: enp0s8: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
```

Now, configure that new interface (Ubuntu uses [netplan](https://netplan.io/) for that):

```plain
multipass exec -- primary sudo bash -c "cat > /etc/netplan/60-bridge.yaml" <<EOF
network:
  ethernets:
    enp0s8:                  # this is the interface name from above
      dhcp4: true
      dhcp4-overrides:       # this is needed so the default gateway
        route-metric: 200    # remains with the first interface
  version: 2
EOF

multipass exec primary sudo netplan apply
```

Finally, find the IP of the instance given by your router:

```plain
multipass exec primary ip address show dev enp0s8 up       
```

For example:

```plain
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:2a:5f:55 brd ff:ff:ff:ff:ff:ff
    inet <b>10.2.0.39</b>/24 brd 10.2.0.255 scope global dynamic enp0s8
       valid_lft 86119sec preferred_lft 86119sec
    inet6 fe80::a00:27ff:fe2a:5f55/64 scope link 
       valid_lft forever preferred_lft forever
```

All the services running inside the instance should now be available on your physical network under http://&lt;instance IP&gt;/.

[/tab]

[tab version="Windows"]

This option only applies to macOS systems.

[/tab]

[/tabs]