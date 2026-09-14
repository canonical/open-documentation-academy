> See also: [Driver](/t/28410), [`local.driver`](/t/27357)

This document demonstrates how to set up port forwarding to a Multipass instance.

## Use VirtualBox to set up port forwarding to a Multipass instance

[tabs]

[tab version="Linux"]

This option only applies to macOS and Windows systems.

[/tab]

[tab version="macOS"]

To expose a service running inside the instance on your host, you can use [VirtualBox's port forwarding feature](https://www.virtualbox.org/manual/ch06.html#natforward).

Example:

```plain
sudo VBoxManage controlvm "primary" natpf1 "myservice,tcp,,8080,,8081"
```

You can then open, say, http://localhost:8081/, and the service running inside the instance on port 8080 will be exposed.

[/tab]

[tab version="Windows"]

To expose a service running inside the instance on your host, you can use [VirtualBox's port forwarding feature](https://www.virtualbox.org/manual/ch06.html#natforward).

Example:

```powershell
& $env:USERPROFILE\Downloads\PSTools\PsExec.exe -s $env:VBOX_MSI_INSTALL_PATH\VBoxManage.exe controlvm "primary" natpf1 "myservice,tcp,,8080,,8081"
```

You can then open, say, http://localhost:8081/, and the service running inside the instance on port 8080 will be exposed.

[/tab]

[/tabs]