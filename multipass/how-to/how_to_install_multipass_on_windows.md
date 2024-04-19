# How to install Multipass on Windows

> See also: [How to use Virtualbox in Multipass on Windows](/t/16626)

**Contents:**

- [Check prerequisites](#heading--check-prerequisites)
  - [Hyper-V](#heading--hyper-v)
  - [VirtualBox](#heading--virtualbox)
- [Install, upgrade, uninstall](#heading--install-upgrade-uninstall)
- [Run](#heading--run)

<a href="#heading--check-prerequisites"><h2 id="heading--check-prerequisites">Check prerequisites</h2></a>

<a href="#heading--hyper-v"><h3 id="heading--hyper-v">Hyper-V</h3></a>

Only **Windows 10 Pro** or **Enterprise**, version **1803** ("April 2018 Update") or later is currently supported. It's due to the necessary version of Hyper-V only being available on those versions.

<a href="#heading--virtualbox"><h3 id="heading--virtualbox">VirtualBox</h3></a>

Multipass also supports using VirtualBox as a virtualization provider.  You can download the latest version from [VirtualBox download page](https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html).

<a href="#heading--install-upgrade-uninstall"><h2 id="heading--install-upgrade-uninstall">Install, upgrade, uninstall</h2></a>

Download the latest installer from [here](https://multipass.run/download/windows). You can also get pre-release versions from [our GitHub releases page](https://github.com/CanonicalLtd/multipass/releases/) - it's the `.exe` file. Run the installer and it will guide you through the steps necessary. You will need to allow the installer to gain Administrator privileges.

You will need either Hyper-V enabled (only Windows 10 Professional or Enterprise), or VirtualBox installed.

To upgrade, just [download the latest installer](https://multipass.run/download/windows) and run it.

You will be asked to uninstall the old version, and a second question about whether to remove all data when uninstalling. If you want to _keep your instances_, answer "No" (the default).

<a href="#heading--run"><h2 id="heading--run">Run</h2></a>

Now, to run normal Multipass commands, open either **Command Prompt** (`cmd.exe`) or **PowerShell** as a regular user.  Use `multipass version` to check your version or `multipass launch` to create your first instance.

Multipass defaults to using Hyper-V as its virtualization provider.  If you'd like to use VirtualBox, you can do so with:

```
multipass set local.driver=virtualbox
```

<!--PREVIOUS DRAFT:

## Downloading

To get Multipass for Windows, download the latest installer from [our GitHub releases page](https://github.com/CanonicalLtd/multipass/releases/) - it's the `.exe` file.

## Prerequisites

### Hyper-V

Only **Windows 10 Pro** or **Enterprise**, version **1803** ("April 2018 Update") or later is currently supported. It's due to the right version of Hyper-V only being available on those versions.

### VirtualBox

Multipass also supports using VirtualBox as a virtualization provider.  You can download the latest version [here](https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html).

## Installation

Make sure the network you're connected to is marked *Private* (which really means *Trusted*), otherwise Windows will prevent Multipass from starting. We're working on resolving that issue.

Run the installer and it will guide you through the steps necessary. You will need to allow the installer to gain Administrator privileges.

You will need either Hyper-V enabled (only Windows 10 Professional or Enterprise), or VirtualBox installed.

## First run

Multipass defaults to using Hyper-V as its virtualization provider.  If you'd like to use VirtualBox, start either **Command Prompt** (`cmd.exe`) or **PowerShell** as `Administrator` and run:
```
C:\WINDOWS\system32> multipass set local.driver=virtualbox
```

Now, to run normal Multipass commands, open either **Command Prompt** (`cmd.exe`) or **PowerShell** as your regular user and you can use `multipass launch` to create your first instance.

With `multipass version` you can check which version you have running:

```plain
$ multipass version
multipass  v1.0.0+win
multipassd v1.0.0+win
```

Have a look at [Working with instances](/t/working-with-multipass-instances/8422) for a rundown of the most common steps. We hope you have fun!

## Upgrading<a name="upgrading"></a>

To upgrade, just [download the latest installer](https://multipass.run/download/windows) and run it. 

You will be asked to uninstall the old version, and a second question about whether to remove all data when uninstalling. If you want to _keep your instances_, answer "No" (the default).

-->