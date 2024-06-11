saviq | 2024-02-07 15:25:00 UTC | #1

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

Multipass also supports using VirtualBox as a virtualization provider.  You can download the latest version [here](https://www.oracle.com/technetwork/server-storage/virtualbox/downloads/index.html).


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

-------------------------

mpt | 2019-06-20 14:21:45 UTC | #2

[quote="saviq, post:1, topic:9547"]
Only Windows 10 Pro or Enterprise, build 1803 or later is currently supported. It’s due to the right version of Hyper-V only being available on those versions.
[/quote]

I’m reliably informed that “Multipass will work on [Windows 10] Home as long as VirtualBox is used as the hypervisor”. Perhaps this article can be updated with instructions how to do that.

-------------------------

mpt | 2019-06-21 06:15:16 UTC | #3

[quote="saviq, post:1, topic:9547"]
Windows 10 Pro or Enterprise, build 1803 or later
[/quote]

Separately, if this is retained, it should be _version_ 1803 (“April 2018 Update”). ([Apparently](https://en.wikipedia.org/wiki/Windows_10_version_history), version 1803 was build 17134.)

-------------------------

saviq | 2019-06-24 09:12:30 UTC | #4

I made [this](/t/installing-multipass-for-windows/9547/3) change for now, as our VirtualBox support is a preview for now, and [tricky](/t/instructions-for-enabling-disabling-virtualbox-support-on-windows-and-macos/11043/1) to enable.

Will amend this when we've improved the story around it.

-------------------------

nobuto | 2019-07-18 05:11:50 UTC | #5

It would be nice if this instruction mentions "Make sure Hyper-V is enabled" with a link as https://docs.microsoft.com/en-us/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v since multipass doesn't explicitly report Hyper-V is not enabled:
https://github.com/CanonicalLtd/multipass/issues/921

-------------------------

saviq | 2019-07-18 07:49:17 UTC | #6

Hi @nobuto,

Thanks for reporting - you're totally right that we need to improve it, and I've replied on the issue:

https://github.com/CanonicalLtd/multipass/issues/921#issuecomment-512692319

-------------------------

jkim711 | 2019-11-27 20:05:26 UTC | #7

I am behind a corporate firewall and I use Proxifier at times to connect through the corporate firewall. Based on the Proxifer logs it seems like it is able to download a few KB. but in the command prompt it says connection failed.

[2019.11.27 21:29:24] multipass.exe (64924) *64 - resolve localhost : DNS
[2019.11.27 21:29:24] multipass.exe *64 - localhost:50051 (IPv6) matching Localhost rule : direct connection
[2019.11.27 21:29:24] multipass.exe *64 - localhost:50051 (IPv6) open directly
[2019.11.27 21:30:32] Explorer.EXE (27956) *64 - resolve DESKTOP-JLRP7H9 : DNS
[2019.11.27 21:30:32] Explorer.EXE (27956) *64 - resolve DESKTOP-JLRP7H9 : DNS
[2019.11.27 21:32:18] PCDist.exe - 70.2.140.151:15020 matching Default rule :  using proxy 70.10.15.10:8080 HTTPS
[2019.11.27 21:32:18] PCDist.exe - 70.2.140.151:15020 open through proxy 70.10.15.10:8080 HTTPS
[2019.11.27 21:32:18] PCDist.exe - 70.2.140.151:15020 close, 56 bytes sent, 8 bytes received, lifetime <1 sec
[2019.11.27 21:32:25] multipass.exe *64 - localhost:50051 (IPv6) close, 932 bytes sent, 1468 bytes (1.43 KB) received, lifetime 03:01

-------------------------

jkim711 | 2019-11-27 20:17:40 UTC | #8

I actually ran it at home PC and got the same result

C:\WINDOWS\system32>multipass launch --name foo
launch failed: The following errors occurred:
foo: timed out waiting for response

-------------------------

rockylhotka | 2019-11-27 21:59:02 UTC | #9

I just installed on Win10 and it works well. However, it isn't clear where virtual images are stored. More importantly it isn't clear how (or if) I can get virtual drive images stored on my big RAID drive, or am I stuck with them in some random location?

I'd expect the `info` command to show the local path to the virtual drive image.

-------------------------

jarno | 2019-12-28 00:57:23 UTC | #10

It would be nice to know where the VM images are being stored

-------------------------

volanavlad | 2019-12-28 13:04:16 UTC | #11

How to build Multipass for Windows? I do not find any windows specific code at canonical/multipass github repo. Where is it?

-------------------------

samiksome | 2020-03-12 14:59:17 UTC | #12

I just installed Multipass (1.1.0+win) on Windows 10 Pro (1909, build 18363.693) with Hyper-V, however contrary to install documentation I did not have to set my network connection to Private (It's still Public). Has this issue been resolved?

-------------------------

saviq | 2020-03-17 12:05:53 UTC | #13

A post was split to a new topic: [Moving files to instances on Windows](/t/moving-files-to-instances-on-windows/14853)

-------------------------

voljka | 2020-04-17 06:36:48 UTC | #14

multipass runs under local system account. It stores all data in system's user profile, located here:
C:\Windows\System32\config\systemprofile
If you need to modify some stuff, then you need to work with psexec.exe -s "some commands here", which allows you to execute commands under the local system account.

-------------------------

saviq | 2020-04-30 10:09:09 UTC | #15

2 posts were split to a new topic: [Upgrading Multipass on Windows](/t/upgrading-multipass-on-windows/15778)

-------------------------

malkebu-lan | 2020-05-11 16:38:38 UTC | #16

Question:  How do I know what my credentials are?

-------------------------

townsend | 2020-05-14 19:45:23 UTC | #17

Hi @malkebu-lan,

I'm not sure which credentials you are referring to.  Could you please explain?

-------------------------

saviq | 2020-05-14 19:48:16 UTC | #18

@malkebu-lan, if you're asking about user and password, the user is `ubuntu` without a password. You can  `multipass shell <instance>` ([`shell` docs here](https://multipass.run/docs/shell-command)), and if you'd like to SSH into the instance, import your keys inside, or use [cloud-init's ssh module](https://cloudinit.readthedocs.io/en/latest/topics/modules.html#ssh) to prepopulate.

-------------------------

brandonp123456 | 2020-05-19 12:26:22 UTC | #19

here is the latest version of virtual box according to the official website 6.1.8 here is the link https://www.virtualbox.org/wiki/Downloads

-------------------------

bramcn0745 | 2020-06-13 23:04:31 UTC | #20

Greetings,
    I have had a difficult time trying to figure out how to move multipass off the default location on my C: Drive in my windows 10 pro host.  My solution was to uninstall multipass and reconfigure my hyper-v where I had no other installed VMs to store the VM information at my desired location.

The settings for the location of where configuration and VMs are stored at is found at the "Hyper-V Settings for the host in the Hyper-V manager.  Is it possible to move this elsewhere or to configure it during install. 

Thank you very much for making a great tool.

-------------------------

saviq | 2020-06-15 14:27:47 UTC | #21

Hi @bramcn0745, welcome!

We are planning to add a configure option for this: [canonical/multipass#1215](https://github.com/canonical/multipass/issues/1215), you may want to subscribe there.

-------------------------

kanchanaw | 2020-08-13 23:24:24 UTC | #22

I think it would also be a good idea to indicate or put in as a "Known Issue" that if someone is planning to use the Multipass created VMs on Windows for installing Kubernetes they may run into issues when the instance get reboot. Windows Default Virtual switch assigns a new IP each time you reboot the instance or the Windows gets rebooted. This causes k8s cluster to not function. 

I love Multipass and uses on Ubuntu and Mac with no issues. However sadly on Windows it causes the above issue. 

Possible feature request:

* Provide the ability to select a different virtual switch other than the Default

Provide the ability to select anything Virtual Switch other than Default

-------------------------

kanchanaw | 2020-08-13 23:27:01 UTC | #23

Ignore this comment, looks like someone had already documented the known issue. Thanks and apologies for the repost.

-------------------------

rozzamozza | 2022-03-10 13:27:36 UTC | #24

Still cant find the .exe. there are release notes for 1.8.1 but cant see any sort of download.

-------------------------

sarnold | 2022-05-04 20:08:10 UTC | #25

There's apparently a choco installer too, that might be more convenient for some users: https://community.chocolatey.org/packages/multipass

Thanks

-------------------------

