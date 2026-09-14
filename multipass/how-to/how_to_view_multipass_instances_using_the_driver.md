> See also: [Driver](/t/28410), [`local.driver`](/t/27357)

This document demonstrates how to view Multipass instances using the driver.

Prerequisite: Have a running Multipass instance.

## Use the driver to view Multipass instances

[tabs]

[tab version="Linux"]

You can view instances with libvirt in two ways, using the `virsh` CLI or the [`virt-manager` GUI](https://virt-manager.org/).

To use the `virsh` CLI, run the following command (see [`man virsh`](http://manpages.ubuntu.com/manpages/xenial/man1/virsh.1.html) for a command reference):

```plain
virsh list                             
```

The output will be similar to the following:

```plain      
 Id   Name                   State
--------------------------------------
 1    unaffected-gyrfalcon   running
```
Alternatively, you can use the `virt-manager` GUI:

![obraz|584x344](upload://bFIxA9CPAqBeWjuQxDaZBYBNsZK.png)

[/tab]

[tab version="macOS"]

Multipass runs as the `root` user. To see Multipass instances via the driver, you need to run Virtualbox or `VBoxManage` commands as root.

To see the instances in VirtualBox, use the command:

```plain
sudo VirtualBox
```

![Multipass instances in VirtualBox](upload://mld2SIalX93tc3StGHaqb6OTyDO.png) 

And, to list the instances on the command line, run:

```plain
sudo VBoxManage list vms
```

Sample output:
```plain
"primary" {395d5300-557d-4640-a43a-48100b10e098}
```

[note type="information"]
You can still use the `multipass` client and the system menu icon. Any changes you make to the configuration of the instances in VirtualBox will be persistent. They may not be represented in Multipass commands such as `multipass info`, though.
[/note]

[/tab]

[tab version="Windows"]

Multipass runs as the _System_ account. To see Multipass instances via the driver, you need to run Virtualbox or `VBoxManage` commands as the _System_ account using [`PsExec -s`](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec). 

Download and unpack [PSTools.zip](https://download.sysinternals.com/files/PSTools.zip) in your *Downloads* folder, and in an administrative PowerShell, run:

```powershell
& $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s -i $env:VBOX_MSI_INSTALL_PATH\VirtualBox.exe
```

![Multipass instances in VirtualBox](upload://xVIOErbwcdoppcN5KSzvHJybSi7.png) 

To list the instances on the command line:

```powershell
& $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s $env:VBOX_MSI_INSTALL_PATH\VBoxManage.exe list vms
```

Sample output:
```powershell
"primary" {05a04fa0-8caf-4c35-9d21-ceddfe031e6f}
```

[note type="information]
You can still use the `multipass` client and the system menu icon. Any changes you make to the configuration of the instances in VirtualBox will be persistent. They may not be represented in Multipass commands such as `multipass info`, though.
[/note]

[/tab]

[/tabs]

