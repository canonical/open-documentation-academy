# How to install Multipass on macOS

> See also: [How to use VirtualBox in Multipass on macOS](/t/16591), [How to use a different terminal on macOS](/t/14955)

**Contents:**

- [Check prerequisites](#heading--check-prerequisites)
- [Install, upgrade, uninstall](#heading--install-upgrade-uninstall)
  - [Using the installer package](#heading--use-the-installer-package)
  - [Using `brew`](#heading--use-brew)
- [Run](#heading--run)

<a href="#heading--check-prerequisites"><h2 id="heading--check-prerequisites">Check prerequisites</h2></a>

<!--### Hypervisor.framework / hyperkit-->

The default backend on macOS is `qemu`, wrapping Apple's Hypervisor.framework. You can use any Mac (M1, M2, or Intel based) with _macOS 10.15 Catalina_ or later installed.

<a href="#heading--install-upgrade-uninstall"><h2 id="heading--install-upgrade-uninstall">Install, upgrade, uninstall</h2></a>

To install Multipass on macOS, you have two options: the installer package or [`brew`](https://brew.sh/). Upgrading and uninstallation options depend on this choice as well.

<a href="#heading--use-the-installer-package"><h3 id="heading--use-the-installer-package">Using the installer package</h3></a>

Download the latest installer from [our download page](https://multipass.run/download/macos). You can also get pre-release versions from [our GitHub releases page](https://github.com/canonical/multipass/releases/) - it's the `.pkg` package.

If you want Tab completion on the command line, install [`bash-completion@2` from `brew`](https://formulae.brew.sh/formula/bash-completion@2) and follow these [instructions](https://docs.brew.sh/Shell-Completion) on updating your `.bashrc` file.

Activate the downloaded installer and it will guide you through the necessary steps. You will need an account with administrator privileges to complete the installation.

![Multipass installer on macOS|658x475](upload://oBqM17GtMd6dzpBJ2OWl3fexIP2.png)

To uninstall, run the script:

```plain
sudo sh "/Library/Application Support/com.canonical.multipass/uninstall.sh"
```

<a href="#heading--use-brew"><h3 id="heading--use-brew">Using `brew`</h3></a>

If you don't have it already, [install Brew](https://brew.sh/). Then, to install Multipass simply execute:

```plain
brew install multipass
```

To uninstall while keeping your VMs and data, you can use:

```plain
brew uninstall multipass
```

[note type="caution"]
*_Caveats:_

Although Brew supports the `--zap` option to remove all data, it does not allow Multipass to remove all the VMs and data properly. The Multipass daemon needs to be alive and running to unregister VMs from certain backends (e.g. VirtualBox). However, Brew executes the zap procedure strictly after the uninstall step. By then, the Multipass daemon is no longer available.

[This issue report](https://github.com/Homebrew/homebrew-cask/issues/85498) has more information. The workaround is to remove VMs manually _before_ uninstalling:

1. `multipass delete --purge --all`  
2. `brew uninstall --zap multipass # to destroy all other data, too`
[/note]

<a href="#heading--run"><h2 id="heading--run">Run</h2></a>

You’ve installed Multipass. Time to run your first commands! Use `multipass version` to check your version or `multipass launch` to create your first instance.

<!--PREVIOUS DRAFT:
## Prerequisites

### Hypervisor.framework / hyperkit

The default backend on macOS is `hyperkit` on Intel, and `qemu` on the M1, wrapping Apple's Hypervisor.framework. You can use any M1 Mac, or a _2010_ or newer Intel Mac with _macOS 10.14 Mojave_ or later installed.

## Installation

To install Multipass on macOS, you have two options: the installer package or [`brew`](https://brew.sh/):

### Installer

Download the latest installer from [our GitHub releases page](https://github.com/CanonicalLtd/multipass/releases/) - it's the `.pkg` package.

If you want Tab completion on the command line, install [`bash-completion` from `brew`](https://formulae.brew.sh/formula/bash-completion) first.

Activate the downloaded installer and it will guide you through the steps necessary. You will need an account with Administrator privileges to complete the installation.

![Multipass installer on macOS|658x475](upload://oBqM17GtMd6dzpBJ2OWl3fexIP2.png)

There's a script to uninstall:
```plain
$ sudo sh "/Library/Application Support/com.canonical.multipass/uninstall.sh"
```

### Brew
Have a look at [`brew.sh`](https://brew.sh/) on instructions to install Brew itself. Then, it's a simple:
```plain
$ brew install --cask multipass
```

To uninstall:
```plain
$ brew uninstall multipass
# or, to destroy all data
$ brew uninstall --zap multipass
```

## First run

Once installed, open the **Terminal** app and you can use `multipass launch` to create your first instance.

With `multipass version` you can check which version you have running:

```plain
$ multipass version
multipass 1.9.0+mac
multipassd 1.9.0+mac
```

Have a look at [Working with instances](/t/working-with-multipass-instances/8422) to quickly get off the ground!

-->