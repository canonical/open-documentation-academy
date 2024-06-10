saviq | 2024-04-09 11:52:50 UTC | #1

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

Download the latest installer from [here](https://multipass.run/download/macos). You can also get pre-release versions from [our GitHub releases page](https://github.com/canonical/multipass/releases/) - it's the `.pkg` package.


If you want Tab completion on the command line, install [`bash-completion@2` from `brew`](https://formulae.brew.sh/formula/bash-completion@2) and follow these [instructions](https://docs.brew.sh/Shell-Completion) on updating your `.bashrc` file.

Activate the downloaded installer and it will guide you through the necessary steps. You will need an account with administrator privileges to complete the installation.

![Multipass installer on macOS|658x475](upload://oBqM17GtMd6dzpBJ2OWl3fexIP2.png)

To uninstall, run the script:
```plain
$ sudo sh "/Library/Application Support/com.canonical.multipass/uninstall.sh"
```

<a href="#heading--use-brew"><h3 id="heading--use-brew">Using `brew`</h3></a>

If you don't have it already, [install Brew](https://brew.sh/). Then, to install Multipass simply execute:

```plain
$ brew install multipass
```

To uninstall while keeping your VMs and data, you can use:

```plain
$ brew uninstall multipass
```

[note type="caution"]
**Caveats:**
Although Brew supports the `--zap` option to remove all data, it does not allow Multipass to remove all the VMs and data properly. The Multipass daemon needs to be alive and running to unregister VMs from certain backends (e.g. VirtualBox). However, Brew executes the zap procedure strictly after the uninstall step. By then, the Multipass daemon is no longer available.

[This issue report](https://github.com/Homebrew/homebrew-cask/issues/85498) has more information. The workaround is to remove VMs manually *before* uninstalling:

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

-------------------------

alexsartin | 2019-06-29 03:20:41 UTC | #2

How do you uninstall multipass on Mac OS?

-------------------------

shuuji3 | 2019-10-07 15:17:32 UTC | #3

If we have [Homebrew](https://brew.sh/), we can install/uninstall multipass with the following commands:

**Install**

    $ brew cask install multipass
    $ multipass version
    multipass  0.8.0+mac
    multipassd 0.8.0+mac

**Uninstall**

    $ brew cask uninstall multipass

-------------------------

saviq | 2019-10-08 16:49:43 UTC | #4

Thanks @shuuji3 I've now incorporated that into the main post above.

-------------------------

shuuji3 | 2019-10-09 00:39:28 UTC | #5

Thanks for improving the documentation!
It should help many developers. :slight_smile:

-------------------------

henryschreineriii | 2019-12-16 14:56:56 UTC | #6

By the way,  you've left off the "cask" part of the uninstall commands, so they don't work, and give an "Error: Unknown command: zap" message, which isn't very helpful. It's `brew cask uninstall multipass` and `brew cask zap multipass`. Otherwise, thanks for the info!

-------------------------

saviq | 2019-12-18 00:07:30 UTC | #7

Hi @henryschreineriii sorry about that, thanks for noticing! Updated the post.

-------------------------

sven | 2019-12-18 19:33:03 UTC | #8

[quote="saviq, post:1, topic:8329"]
multipass version
[/quote]

Hi, many thanks for the tool and documentation.

Two things come to my mind:

1. Could you provide information here (as on the [Windows installation page](https://discourse.ubuntu.com/t/installing-multipass-for-windows/9547)) which prerequisites, especially virtualisation methods are used/available?

2. With the 1.0 release of today, can you update the output of `multipass version` at this page?

Thanks

-------------------------

saviq | 2019-12-18 21:08:08 UTC | #9

Hi @sven, I've now made the changes you mentioned. Thanks for prompting :).

-------------------------

saviq | 2020-02-07 09:22:54 UTC | #10

3 posts were split to a new topic: [Minimum macOS version for Multipass?](/t/minimum-macos-version-for-multipass/14301)

-------------------------

tgwaste | 2020-03-06 17:52:13 UTC | #12

If you did not use brew to install you can uninstall from macOS like this:

sudo sh "/Library/Application Support/com.canonical.multipass/uninstall.sh"

-------------------------

ya-bo-ng | 2021-03-09 16:30:23 UTC | #13

We have [received an issue](https://github.com/canonical-web-and-design/multipass.run/issues/120) on the website about this content.

TLDR;
Homebrew sends GA events. Which can be disabled but the user needs to know-how. I would suggest adding a small comment with a link to docs to disable analytics.

-------------------------

apagiaro | 2020-12-08 10:42:59 UTC | #14

Is there a way to install multipass from the source code on macOS?

-------------------------

undefynd | 2020-12-11 00:16:08 UTC | #15

The `brew` command you guys specify in the documentation is outdated.

to install run
> brew install --cask multipass

-------------------------

ricab | 2020-12-11 10:39:32 UTC | #16

Hi @apagiaro, we're keeping some macOS and Windows parts closed-source for now, so no, we don't currently provide a way to build or install from source on those platforms.

-------------------------

ricab | 2020-12-11 10:41:19 UTC | #17

Hi @undefynd, thanks for point that out. I have just updated those commands.

-------------------------

williamli | 2020-12-30 10:45:48 UTC | #18

I think the uninstall command to zap is 

    brew uninstall --zap --cask multipass

-------------------------

saviq | 2021-01-09 09:30:04 UTC | #19

[quote="williamli, post:18, topic:8329"]
`brew uninstall --zap --cask multipass`
[/quote]

Doesn't look it:

```
Error: invalid option: --zap
```

-------------------------

leon-mintz | 2023-02-10 08:36:34 UTC | #20

After installing with `brew` and adding my user to the admin group, I still need a `sudo` to use multipass, otherwise
```
$ multipass shell charm-dev-29                                                                                                                                                      
shell failed: The client is not authenticated with the Multipass service.
Please use 'multipass authenticate' before proceeding.
```
And authenticate doesn't seem to work: the passphrase gets rejected:
```
$ sudo multipass authenticate
Password:
Please enter passphrase: 
authenticate failed: Passphrase is not set. Please `multipass set local.passphrase` with a trusted client.
```
What's a trusted client?

-------------------------

ricab | 2023-02-10 12:43:44 UTC | #21

Hey @leon-mintz, have a look at at these docs on [security](https://multipass.run/docs/about-security) and [how to authenticate](https://multipass.run/docs/authenticating-clients). The daemon will only deal with clients/users that it trusts. On Linux, that's a) the first user belonging to the `sudo` group (+ `root`) who connects (presumably the one who installed the application) and b) clients that were able to authenticate with `multipass authenticate` thereafter. 

So, when an administrator who installs multipass wants to allow others, he or she needs to set a password and share it with allowed users, who then need to `multipass authenticate` with it. You probably connected as root the first time, so only root is trusted by `multipassd`.

-------------------------

townsend | 2023-02-10 12:54:05 UTC | #22

Hi @leon-mintz!
 
I will add that since `sudo multipass ...` works, this indicates that you used `sudo multipass ...` first, so `root` is the trusted user, not your own user.  Also, since `sudo` is the trusted user, you will need to do the following:
```plain
$ sudo multipass set local.passphrase
Please enter passphrase:
Please re-enter passphrase:
$ multipass authenticate
Please enter passphrase:
```

Also, it's not advised to run `multipass` with `sudo`.  Is there a reason why you've done that?

-------------------------

leon-mintz | 2023-02-10 15:17:27 UTC | #23

[quote="townsend, post:22, topic:8329"]
it’s not advised to run `multipass` with `sudo`. Is there a reason why you’ve done that?
[/quote]

I was using `sudo` because my user account doesn't have permissions in `/opt/brew`:
```bash
$ brew install --cask multipass
Error: /opt/homebrew is not writable. You should change the
ownership and permissions of /opt/homebrew back to your
user account:
  sudo chown -R $(whoami) /opt/homebrew
```

[quote="ricab, post:21, topic:8329"]
You probably connected as root the first time, so only root is trusted by `multipassd`.
[/quote]

Exactly.

Thank you!

-------------------------

garyKrause | 2023-04-12 14:21:07 UTC | #24

I just installed multipass using homebrew
```
└─(10:03:56)──> brew install --cask multipass                                                                                                                                                                                
Warning: Cask 'multipass' is already installed.

To re-install multipass, run:
  brew reinstall --cask multipass
```

but it appears it's not part of my PATH
```
└─(10:06:16)──> multipass                                                                                                                                                                                                          
zsh: command not found: multipass
```

I have tried restarting my shell as well with same results.

Edit: I installed with the package and had the same results. I am on Ventura 13.3 (22E252) using a 2021 MBP 16in M1 w/ 32GB RAM. Are there additionaly steps I'm missing?

-------------------------

bainmckay | 2023-09-15 18:36:17 UTC | #25

Hi can I move a multipass VM repository to another disk and run it?  I know I can move the files (instance .iOS and .img) to another disk as a way to snapshot it. But can I move a VM and run it from another drive

-------------------------

georgeliaojia | 2023-09-18 18:20:11 UTC | #26

Hi @bainmckay , if I did not understand wrong, you want to move one VM instance directory instead of all multipassd data  to another drive. Multipass CLI do not have such a support but maybe you can use create symbolic link (ln -s <old_directory> <new_directory>) between the new and old directory as a workaround.

-------------------------

bainmckay | 2023-11-03 07:12:57 UTC | #27

Yes. That may work. Thank you.

-------------------------

