> See also: [Driver](/t/28410), [`local.driver`](/t/27357)

This document demonstrates how to change to an alternative virtualisation driver and how to switch back to default driver.

Be aware that Multipass already has sensible defaults, so this is an optional step. 

To review the default drivers and learn how to choose one, see: [Driver](/t/28410). 

## Configure an alternative virtualisation driver

Please note that changing your driver will also change its hypervisor.

[tabs]

[tab version="Linux"]

If you want to (or have to), you can switch to the experimental [libvirt](https://libvirt.org/) instead of using the default hypervisor that Multipass uses.

To install libvirt, run the following command (or use the equivalent for your Linux distribution):

```plain
sudo apt install libvirt-daemon-system
```

Now you can switch the Multipass driver to libvirt. First, enable Multipass to use your local libvirt by connecting to the libvirt interface/plug:

```plain
sudo snap connect multipass:libvirt
```

Then, stop all instances and tell Multipass to use libvirt, running the following commands:

```plain
multipass stop --all
multipass set local.driver=libvirt
```

All your existing instances will be migrated and can be used straight away.

[note type="information"]
You can still use the `multipass` client and the tray icon, and any changes you make to the configuration of the instance in libvirt will be persistent. They may not be represented in Multipass commands such as `multipass info`, though.
[/note]

[/tab]

[tab version="macOS"]

If you want to (or have to), you can switch to VirtualBox instead of using the default hypervisor that Multipass uses. 

To switch the Multipass driver to VirtualBox, run this command:

```plain
sudo multipass set local.driver=virtualbox
```

From now on, all instances started with `multipass launch` will use VirtualBox behind the scenes.

[/tab]

[tab version="Windows"]

If you want to (or have to), you can switch to VirtualBox instead of using the default hypervisor that Multipass uses. 

To that end, you need to install VirtualBox and to [run the VirtualBox installer as administrator](https://forums.virtualbox.org/viewtopic.php?f=6&t=88405#p423658). 

To switch the Multipass driver to VirtualBox (also with Administrator privileges), run this command:

```powershell
multipass set local.driver=virtualbox
```

From then on, all instances started with `multipass launch` will use VirtualBox behind the scenes.

[/tab]

[/tabs]

## Switch back to the default driver

> See also: [`stop`](/t/23951), [`local.driver`](/t/27357)

[tabs]

[tab version="Linux"]

To switch back to the default `qemu` driver, run the following commands:

```plain
multipass stop --all
multipass set local.driver=qemu
```

All your existing instances will be migrated and can be used straight away.

[note type="information"]
This will make you lose any customisations you made to the instance in libvirt.
[/note]

[/tab]

[tab version="macOS"]

If you want to switch back to the default driver, run the following command:

```plain
multipass set local.driver=qemu
```

[note type="information"]
Instances created with VirtualBox don't get transferred, but you can always come back to them.
[/note]

[/tab]

[tab version="Windows"]

If you want to switch back to the default driver, run the following command:

```plain
multipass set local.driver=hyperv
```
[note type="information"]
Instances created with VirtualBox don't get transferred, but you can always come back to them.
[/note]

[/tab]

[/tabs]

---

*Errors or typos? Topics missing? Hard to read? <a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/set-up-the-driver" target="_blank">Let us know</a> or <a href="https://github.com/canonical/multipass/issues/new/choose" target="_blank">open an issue on GitHub</a>.*
