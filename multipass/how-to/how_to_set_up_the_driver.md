tmihoc | 2024-07-10 09:07:46 UTC | #1

<!-- New feedback link at the top of each page!
Please don't copy it blindly, first update the URL passed to the form with the current page URL 
-->

> *Errors or typos? Topics missing? Hard to read? <a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/set-up-the-driver" target="_blank">Let us know</a> or <a href="https://github.com/canonical/multipass/issues/new/choose" target="_blank">open an issue on GitHub</a>.*

> See also: [Driver](/t/28410), [`local.driver`](/t/27357)

This document demonstrates how to choose, set up, and manage the drivers behind Multipass. Multipass already has sensible defaults, so this is an optional step. 

## Default driver

[tabs]

[tab version="Linux"]

By default, Multipass on Linux uses the `qemu` or `lxd` driver (depending on the architecture). 

[/tab]

[tab version="macOS"]

By default, Multipass on macOS uses the `qemu` driver. 

[/tab]

[tab version="Windows"]

By default, Multipass on Windows uses the `hyperv` driver. 

[/tab]

[/tabs]

## Install an alternative driver

[tabs]

[tab version="Linux"]

If you want more control over your VMs after they are launched, you can also use the experimental [libvirt](https://libvirt.org/) driver. 

To install libvirt, run the following command (or use the equivalent for your Linux distribution):

```plain
$ sudo apt install libvirt-daemon-system
```

Now you can switch the Multipass driver to libvirt. First, allow Multipass to use your local libvirt by connecting to the libvirt interface/plug:

```plain
$ sudo snap connect multipass:libvirt
```

Then, [stop](/t/23951) all instances and tell Multipass to use libvirt, running the following commands:

```plain
$ multipass stop --all
$ multipass set local.driver=libvirt
```

All your existing instances will be migrated and can be used straight away.

[note type="information"]
You can still use the `multipass` client and the tray icon, and any changes you make to the configuration of the instance in libvirt will be persistent. They may not be represented in Multipass commands such as `multipass info`, though.
[/note]

[/tab]

[tab version="macOS"]

An alternative option is to use VirtualBox.

To switch the Multipass driver to VirtualBox, run this command:

```plain
$ sudo multipass set local.driver=virtualbox
```

From now on, all instances started with `multipass launch` will use VirtualBox behind the scenes.

[/tab]

[tab version="Windows"]

If you want to (or have to), you can change the hypervisor that Multipass uses to VirtualBox. 

To that end, install VirtualBox, if you haven't yet. You may find that you need to [run the VirtualBox installer as administrator](https://forums.virtualbox.org/viewtopic.php?f=6&t=88405#p423658). 

To switch the Multipass driver to VirtualBox (also with Administrator privileges), run this command:

```powershell
PS> multipass set local.driver=virtualbox
```

From then on, all instances started with `multipass launch` will use VirtualBox behind the scenes.

[/tab]

[/tabs]

## Use the driver to view Multipass instances

[tabs]

[tab version="Linux"]

You can view instances with libvirt in two ways, using the `virsh` CLI or the [`virt-manager` GUI](https://virt-manager.org/).

To use the `virsh` CLI, launch an instance and then execute `virsh list` (see [`man virsh`](http://manpages.ubuntu.com/manpages/xenial/man1/virsh.1.html) for a command reference):

```plain
$ virsh list                             
 Id   Name                   State
--------------------------------------
 1    unaffected-gyrfalcon   running
```

Alternatively, to use the `virt-manager` GUI, ...

![obraz|584x344](upload://bFIxA9CPAqBeWjuQxDaZBYBNsZK.png)

[/tab]

[tab version="macOS"]

Multipass runs as the `root` user, so to see the instances in  VirtualBox, or through the `VBoxManage` command, you have to run those as `root`, too. To see the instances in VirtualBox, execute:

```plain
$ sudo VirtualBox
```

![Multipass instances in VirtualBox](upload://mld2SIalX93tc3StGHaqb6OTyDO.png) 

And, to list the instances on the command line, execute:

```plain
$ sudo VBoxManage list vms
"primary" {395d5300-557d-4640-a43a-48100b10e098}
```

[note type="information"]
You can still use the `multipass` client and the system menu icon, and any changes you make to the configuration of the instances in VirtualBox will be persistent. They may not be represented in Multipass commands such as `multipass info` , though.
[/note]

[/tab]

[tab version="Windows"]

Multipass runs as the _System_ account, so to see the instances in VirtualBox, or through the `VBoxManage` command, you have to run those as that user via [`PsExec -s`](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec). Download and unpack [PSTools.zip](https://download.sysinternals.com/files/PSTools.zip) in your *Downloads* folder, and in an administrative PowerShell, run:

```powershell
PS> & $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s -i $env:VBOX_MSI_INSTALL_PATH\VirtualBox.exe
```

![Multipass instances in VirtualBox](upload://xVIOErbwcdoppcN5KSzvHJybSi7.png) 

To list the instances on the command line:
```powershell
PS> & $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s $env:VBOX_MSI_INSTALL_PATH\VBoxManage.exe list vms
"primary" {05a04fa0-8caf-4c35-9d21-ceddfe031e6f}
```

[note type="information]
You can still use the `multipass` client and the system menu icon, and any changes you make to the configuration of the instances in VirtualBox will be persistent. They may not be represented in Multipass commands such as `multipass info`, though.
[/note]

[/tab]

[/tabs]


## Use VirtualBox to set up port forwarding for a Multipass instance

[tabs]

[tab version="Linux"]

This option only applies to macOS and Windows systems.

[/tab]

[tab version="macOS"]

To expose a service running inside the instance on your host, you can use [VirtualBox's port forwarding feature](https://www.virtualbox.org/manual/ch06.html#natforward), for example:

```plain
$ sudo VBoxManage controlvm "primary" natpf1 "myservice,tcp,,8080,,8081"
```

You can then open, say, http://localhost:8081/, and the service running inside the instance on port 8080 will be exposed.

[/tab]

[tab version="Windows"]

To expose a service running inside the instance on your host, you can use [VirtualBox's port forwarding feature](https://www.virtualbox.org/manual/ch06.html#natforward), for example:

```powershell
PS> & $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s $env:VBOX_MSI_INSTALL_PATH\VBoxManage.exe controlvm "primary" natpf1 "myservice,tcp,,8080,,8081"
```

You can then open, say, http://localhost:8081/, and the service running inside the instance on port 8080 will be exposed.

[/tab]

[/tabs]
 
## Use VirtualBox to set up network bridging for a Multipass instance 

[tabs]

[tab version="Linux"]

This option only applies to macOS systems.

[/tab]

[tab version="macOS"]

An often requested Multipass feature is network bridging. You can add a second network interface to the instance and expose it on your physical network. 

First, stop the instance:

```plain
$ multipass stop primary
```

Now, find the network interface you want to bridge with (you want the identifier before the second colon):

```plain
$ VBoxManage list bridgedifs | grep ^Name:
Name:            <b>en0</b>: Ethernet
Name:            en1: Wi-Fi (AirPort)
Name:            en2: Thunderbolt 1
Name:            en3: Thunderbolt 2
...
```

Finally, tell VirtualBox to use it as the "parent" for the second interface (see more info on bridging in [VirtualBox documentation](https://www.virtualbox.org/manual/ch06.html#network_bridged) on this topic):

```plain
$ sudo VBoxManage modifyvm primary --nic2 bridged --bridgeadapter2 en0
```

[note=caution]
Do not touch --nic1 as that's used by Multipass.
[/note]

You can then start the instance again and find the name of the new interface:

```plain
$ multipass start primary
$ multipass exec primary ip link | grep DOWN
3: <b>enp0s8</b>: <BROADCAST,MULTICAST> mtu 1500 qdisc noop state DOWN mode DEFAULT group default qlen 1000
```

And configure that new interface — Ubuntu uses [netplan](https://netplan.io/) for that:

```plain
$ multipass exec -- primary sudo bash -c "cat > /etc/netplan/60-bridge.yaml" <<EOF
network:
  ethernets:
    enp0s8:                  # this is the interface name from above
      dhcp4: true
      dhcp4-overrides:       # this is needed so the default gateway
        route-metric: 200    # remains with the first interface
  version: 2
EOF
$ multipass exec primary sudo netplan apply
```

Finally, find the IP of the instance given by your router:

```plain
$ multipass exec primary ip address show dev enp0s8 up       
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:2a:5f:55 brd ff:ff:ff:ff:ff:ff
    inet <b>10.2.0.39</b>/24 brd 10.2.0.255 scope global dynamic enp0s8
       valid_lft 86119sec preferred_lft 86119sec
    inet6 fe80::a00:27ff:fe2a:5f55/64 scope link 
       valid_lft forever preferred_lft forever
```

All the services running inside the instance should now be available on your physical network under http://&lt;the ip&gt;/.

[/tab]

[tab version="Windows"]

This option only applies to macOS systems.

[/tab]

[/tabs]

## Switch back to the default driver

> See also: [`stop`](/t/23951), [`local.driver`](/t/27357)

[tabs]

[tab version="Linux"]

To switch back to the default `qemu` driver, first you need to stop all instances again:

```plain
$ multipass stop --all
$ multipass set local.driver=qemu
```

Here, too, existing instances will be migrated.

[note type="information"]
This will make you lose any customisations you made to the instance in libvirt.
[/note]

[/tab]

[tab version="macOS"]

If you want to switch back to the default driver, run:

```plain
$ sudo multipass set local.driver=qemu
```

Instances created with VirtualBox don't get transferred, but you can always come back to them.

[/tab]

[tab version="Windows"]

If you want to switch back to the default driver:

```plain
PS> multipass set local.driver=hyperv
```

Instances created with VirtualBox don't get transferred, but you can always come back to them.

[/tab]

[/tabs]


---
*<a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/set-up-the-driver" target="_blank">Let us know</a> how this worked for you and what you’d like to see next!*

-------------------------