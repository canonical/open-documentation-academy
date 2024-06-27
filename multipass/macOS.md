nhart | 2023-08-23 15:59:54 UTC | #1

Multipass is a flexible and powerful tool that can be used for many purposes. In its simplest form, it can be used to quickly create and destroy Ubuntu VMs (instances) on any host machine. When used maximally, Multipass is a local mini-cloud on your laptop, ensuring that you can test and develop multi-instance or container-based cloud applications.

This tutorial will teach you how to create, customise and manage instances using Multipass. You will also learn how to apply Multipass in two common use cases.


## Contents
- [Install Multipass](#heading--install-multipass)
- [Create and use a basic instance](#heading--create-and-use-a-basic-instance)
- [Create a customised instance](#heading--create-a-customised-instance)
- [Manage instances](#heading--manage-instances)
- [Put your instances to use](#heading--put-your-instances-to-use)
  - [Run a simple web server](#heading--run-a-simple-web-server)
  - [Launch from a Blueprint to run Docker containers](#heading--launch-from-a-blueprint-to-run-docker-containers)
- [Next steps](#heading--next-steps)


<a href="#heading--install-multipass"><h2 id="heading--install-multipass">Install Multipass</h3></a>

Multipass is available for Linux, macOS and Windows. To install it on the OS of your choice, please follow the instructions given in this [how-to-guides](https://multipass.run/docs/how-to-install-multipass). 

This tutorial gives instructions for using Multipass on macOS.

<a href="#heading--create-and-use-a-basic-instance"><h2 id="heading--create-and-use-a-basic-instance">Create and use a basic instance</h2></a>

<!--
The easiest way to use Multipass is with the primary instance.
Linux,
The primary instance is a Multipass virtual machine that is configured to be useful for generic purposes out of the box.

The primary instance automatically mounts the $HOME directory (files in this directory are shared between the host and the instance), and it comes with Multipass’s default specs: 1GB of RAM, 5GB of disk, and 1 CPU.
-->

Start Multipass from the application launcher. In macOS, open the application launcher, type "Multipass", and launch the application.

**![|720x451](https://assets.ubuntu.com/v1/a4ff7fad-mp-macos-1.png)**

After launching the application, you should see the Multipass tray icon in the upper right section of the screen.

**![|684x48](https://assets.ubuntu.com/v1/42bb4ccb-mp-macos-2.png)**

Click on the Multipass icon and select **Open Shell**. 

**![|304x352](https://assets.ubuntu.com/v1/eb92083a-mp-macos-3.png)**

Clicking this button does many things in the background. First, it creates a new virtual machine (instance) named "primary", with 1GB of RAM, 5GB of disk, and 1 CPU. Second, it installs the most recent Ubuntu LTS release on that instance. Third, it mounts your `$HOME` directory in the instance. Last, it opens a shell to the instance, announced by the command prompt `ubuntu@primary`. 

You can see elements of this in the printout below:

```plain
Launched: primary                                                               
Mounted '/home/<user>' into 'primary:Home'                                       
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-57-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jan 26 21:15:05 UTC 2023

  System load:  0.72314453125     Processes:             105
  Usage of /:   29.8% of 4.67GB   Users logged in:       0
  Memory usage: 20%               IPv4 address for ens3: 192.168.64.5
  Swap usage:   0%


0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

ubuntu@primary:~$
```

Let’s test it out. As you've just learnt, the previous step automatically mounted your `$HOME` directory in the instance. Use this to share data with your instance. More concretely, create a new folder called `Multipass_Files` in your `$HOME` directory.

**![|720x329](https://assets.ubuntu.com/v1/5867fb49-mp-macos-4.png)**

As you can see, a `README.md` file has been added to the shared folder. Check for the folder and read the file from your new instance:

```plain
ubuntu@primary:~$ cd ./Home/Multipass_Files/
ubuntu@primary:~/Home/Multipass_Files$ cat README.md 
## Shared Folder

This folder could be a great place to keep files that need to be accessed by both your host machine and Ubuntu VM!

ubuntu@primary:~/Home/Multipass_Files$
```

Congratulations, you've got your first instance! 

This instance is great for when you just need a quick Ubuntu VM, but let's say you want a more customised instance, how can you do that? Multipass has you covered there too.

[details = "Optional Exercises"]
Exercise 1: 
When you select Open Shell, what happens in the background is the equivalent of the CLI commands `multipass launch –name primary`  followed by  `multipass shell`. Open a terminal and try `multipass shell` (if you didn't follow the steps above, you will have to run the `launch` command first).

Exercise 2: 
In Multipass, an instance with the name "primary" is privileged. That is, it serves as the default argument of `multipass shell` among other capabilities. In different terminal instances, check `multipass shell primary` and `multipass shell`. Both commands should give the same result.
[/details]

<a href="#heading--create-a-customised-instance"><h2 id="heading--create-a-customised-instance">Create a customised instance</h3></a>

Multipass has a great feature to help you get started with creating customised instances. Open a terminal and run the `multipass find` command. The result shows a list of all images you can currently launch through Multipass.

```plain
$ multipass find
Image                       Aliases           Version          Description
snapcraft:core18            18.04             20201111         Snapcraft builder for Core 18
snapcraft:core20            20.04             20210921         Snapcraft builder for Core 20
snapcraft:core22            22.04             20220426         Snapcraft builder for Core 22
snapcraft:devel                               20230126         Snapcraft builder for the devel series
core                        core16            20200818         Ubuntu Core 16
core18                                        20211124         Ubuntu Core 18
core20                                        20230119         Ubuntu Core 20
core22                                        20230119         Ubuntu Core 22
18.04                       bionic            20230112         Ubuntu 18.04 LTS
20.04                       focal             20230117         Ubuntu 20.04 LTS
22.04                       jammy,lts         20230107         Ubuntu 22.04 LTS
22.10                       kinetic           20230112         Ubuntu 22.10
daily:23.04                 devel,lunar       20230125         Ubuntu 23.04
appliance:adguard-home                        20200812         Ubuntu AdGuard Home Appliance
appliance:mosquitto                           20200812         Ubuntu Mosquitto Appliance
appliance:nextcloud                           20200812         Ubuntu Nextcloud Appliance
appliance:openhab                             20200812         Ubuntu openHAB Home Appliance
appliance:plexmediaserver                     20200812         Ubuntu Plex Media Server Appliance
anbox-cloud-appliance                         latest           Anbox Cloud Appliance
charm-dev                                     latest           A development and testing environment for charmers
docker                                        0.4              A Docker environment with Portainer and related tools
jellyfin                                      latest           Jellyfin is a Free Software Media System that puts you in control of managing and streaming your media.
minikube                                      latest           minikube is local Kubernetes
```

Launch an instance running Ubuntu 22.10 ("Kinetic Kudu") by typing the `multipass launch kinetic` command.

Now, you have an instance running and it has been named randomly by Multipass. In this case, it has been named "breezy-liger".

```plain
$ multipass launch kinetic
Launched: breezy-liger
```

You can check some basic info about your new instance by running the following command:

`multipass exec breezy-liger -- lsb_release -a`

This tells Multipass to execute the command `lsb_release -a` on the “breezy-liger” instance.

```plain
$ multipass exec breezy-liger -- lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.10
Release:		22.10
Codename:		kinetic
```

Perhaps after using this instance for a while, you decide that what you really need is the latest LTS version of Ubuntu, with a more informative name and a little more memory and disk. You can delete the "breezy-liger" instance by running the following command:

`multipass delete breezy-liger`

You can now launch the type of instance you need by running this command:

`multipass launch lts --name ltsInstance --memory 2G --disk 10G --cpus 2`

<a href="#heading--manage-instances"><h2 id="heading--manage-instances">Manage instances</h3></a>

You can confirm that the new instance has the specs you need by running `multipass info ltsInstance`.

```plain
$ multipass info ltsInstance                             
Name:           ltsInstance
State:          Running
IPv4:           192.168.64.3
Release:        Ubuntu 22.04.1 LTS
Image hash:     3100a27357a0 (Ubuntu 22.04 LTS)
CPU(s):         2
Load:           1.55 0.44 0.15
Disk usage:     1.4GiB out of 9.5GiB
Memory usage:   155.5MiB out of 1.9GiB
Mounts:         --
```

You've created and deleted quite a few instances. It is time to run `multipass list` to see the instances you currently have.

```plain
$ multipass list                                         
Name                    State             IPv4             Image
primary                 Running           192.168.64.5     Ubuntu 22.04 LTS
breezy-liger            Deleted           --               Not Available
ltsInstance             Running           192.168.64.3     Ubuntu 22.04 LTS
```

The result shows that you have two instances running, the "primary" instance and the LTS machine with customised specs. The "breezy-liger" instance is still listed, but its state is "Deleted". You can recover this instance by running `multipass recover breezy-liger`. But for now, delete the instance permanently by running `multipass purge`. Then run `multipass list` again to confirm that the instance has been permanently deleted. 

```plain
$ multipass list                                         
Name                    State             IPv4             Image
primary                 Running           192.168.64.5     Ubuntu 22.04 LTS
ltsInstance             Running           192.168.64.3     Ubuntu 22.04 LTS
```

You've now seen a few ways to create, customise, and delete an instance. It is time to put those instances to work!

<a href="#heading--put-your-instances-to-use"><h2 id="heading--put-your-instances-to-use">Put your instances to use</h2></a>

One way to put a Multipass instance to use is by running a local or web server in it.

<a href="#heading--run-a-simple-web-server"><h3 id="heading--run-a-simple-web-server">Run a simple web server</h3></a>

Return to your customised LTS instance. Take note of its IP address, which was revealed when you ran `multipass list`. Then run `multipass shell ltsInstance` to open a shell in the instance.

From the shell, you can run:

```
sudo apt update

sudo apt install apache2
```

Open a browser and type in the IP address of your instance into the address bar. You should now see the default Apache homepage.

![|720x545](https://assets.ubuntu.com/v1/e106f7f9-mp-macos-5.png)

Just like that, you've got a web server running in a Multipass instance!

You can use this web server locally for any kind of local development or testing. However, if you want to access this web server from the internet (for instance, a different computer), you need an instance that is exposed to the external network.

<a href="#heading--launch-from-a-blueprint-to-run-docker-containers"><h3 id="heading--launch-from-a-blueprint-to-run-docker-containers">Launch from a Blueprint to run Docker containers</h3></a>

Some environments require a lot of configuration and setup. Multipass Blueprints are instances with a deep level of customization. For example, the Docker Blueprint is a pre-configured Docker environment with a Portainer container already running. You can launch an instance using the Docker Blueprint by running `multipass launch docker --name docker-dev`.

Once that's done, run `multipass info docker-dev` to note down the IP of the new instance.

```plain
$ multipass launch docker --name docker-dev
Launched: docker-dev
$ multipass info docker-dev
Name:           docker-dev
State:          Running
IPv4:           10.115.5.235
                172.17.0.1
Release:        Ubuntu 22.04.1 LTS
Image hash:     3100a27357a0 (Ubuntu 22.04 LTS)
CPU(s):         2
Load:           1.40 0.64 0.25
Disk usage:     2.5GiB out of 38.6GiB
Memory usage:   236.4MiB out of 3.8GiB
Mounts:         --
```

Copy the IP address starting with "10" and paste it into your browser, then add a colon and the portainer default port, 9000. It should look like this: 10.115.5.235:9000. This will take you to the Portainer login page where you can set a username and password.

![|720x543](https://assets.ubuntu.com/v1/75a164a1-mp-macos-6.png)

From there, select **Local** to manage a local Docker environment.

![|720x601](https://assets.ubuntu.com/v1/ee3ff308-mp-macos-7.png)

Inside the newly selected local Docker environment, locate the sidebar menu on the page and click on **app templates**, then select **NGINX**.

![|720x460](https://assets.ubuntu.com/v1/86be3eae-mp-macos-8.png)

From the Portainer dashboard, you can see the ports available on nginx. To verify that you have nginx running in a Docker container inside Multipass, open a new web page and paste the IP address of your instance followed by one of the port numbers.

![|720x465](https://assets.ubuntu.com/v1/25585a03-mp-macos-9.png)

<a href="#heading--next-steps"><h2 id="heading--next-steps">Next steps</h2></a>

Congratulations! You can now use Multipass proficiently. There's more to learn about Multipass and its capabilities. Check out our [how-to guides](https://multipass.run/docs/how-to-guides) for ideas and help with your project. Our [reference pages](https://multipass.run/docs/reference) contain definitions of key concepts, a complete CLI command reference, settings options and more.

Let us know what you’re able to get done with Multipass!

-------------------------

kirhist | 2022-10-25 18:12:32 UTC | #2

Hi Nathan,

Thank you for the tutorial, it is very helpful. The only thing is that when i followed it the docker VM that I installed with:
`$ multipass launch docker --name docker-dev`
had no docker and Portainer installed:
> $ multipass info docker-dev                
>     Name:           docker-dev
>     State:          Running
>     IPv4:           192.168.64.6
>     Release:        Ubuntu 22.04.1 LTS
>     Image hash:     f753d6f9cea8 (Ubuntu 22.04 LTS)
>     Load:           0.01 0.07 0.07
>     Disk usage:     1.6G out of 38.6G
>     Memory usage:   173.6M out of 3.8G
>     Mounts:         --
> 
>     $ multipass exec docker-dev docker
>     bash: line 1: docker: command not found

-------------------------

nhart | 2022-10-27 12:08:25 UTC | #3

Hi kirhist,
Thanks for reaching out. I'm not sure why that is happening, but it could be that some part of the cloud-init initialization built into the blueprint is failing. I would try to delete and purge the instance (`multipass delete docker-dev` then `multipass purge`), then try the `launch` command again. If that doesn't work would you mind filing a bug report [here](https://github.com/canonical/multipass/issues)?

Thanks and good luck!

-------------------------

etiquet | 2022-10-29 16:23:41 UTC | #4




Hello very simple to use. Any tips how make the vm accessible from outside the Mac ? Can find dozen tips on linux be not on Mac or bsd. Highly appreciated.

-------------------------

ricab | 2022-10-31 10:55:33 UTC | #5

Hi @etiquet, if you are using either the `qemu` or the `virtualbox` backends, you can [bridge your instance](https://multipass.run/docs/additional-networks) to a network interface on your host. 

You just have to run `multipass networks`, choose a network (e.g. `en0`), and pass it in to the `launch` command (e.g. `multipass launch --network en0`). Let us know how it goes!

-------------------------

qm64 | 2023-10-11 13:16:59 UTC | #6

Hi @nhart,

I'm probably doing it wrong, but after installing, according to the linked instructions, for Mac OS on M1, Sonoma 14.0, I don't get the "Open Shell" option in the app. Here's what I get:

![Multipass_Dock_Options|188x221](upload://9IKsnQgGqvAzGmCS6OnnkcY1gnM.png) 

From there, I didn't have much luck. For instance, running `multipass launch` and `multipass shell`, I can drop into a shell, but it's not in `$HOME`, etc. 

Am I doing it right? Or has MacOS / Multipass moved on from these instructions?

Cheers,
QM

-------------------------

ricab | 2023-10-11 13:59:59 UTC | #7

Hi @qm64, that icon does not appear related to Multipass.

> From there, I didn’t have much luck. For instance, running `multipass launch` and `multipass shell`, I can drop into a shell, but it’s not in `$HOME`, etc.

I'm sorry, I don't understand what you mean. Are you able to successfully get a shell into a VM with `multipass shell`? The VM does not know about your host users or filesystem, so your `$HOME` will be different. But you can mount your home from the host to the instance. Multipass does so automatically when you use the primary instance:

```
$ multipass start
Launched: primary
Mounted '/home/ricab' into 'primary:Home'
$ multipass exec -n primary -- ls Home
[your_stuff]
```

-------------------------

qm64 | 2023-10-11 14:44:46 UTC | #8

Hi @ricab,

Thanks, that's helpful.

Just to close off my previous comments...

I installed `multipass-1.12.2+mac-Darwin.pkg` from the link on the installation page linked from this page.

Here's the app that is then available in Launchpad, named Multipass.app, and matches the icon on the Multipass pages.
[Can't show the Launchpad screenshot, I'm limited to only 1 image due to "new user" limit.]

Here's the app/icon that shows up in the dock immediately after running the app from Launchpad:
![Multipass GUI in Dock|124x132](upload://7BTJMxokpMHAJXKr5TTLgOgPpjV.png) 

I just want to re-iterate -- the instructions to download the installer pkg result in this app / icon, which **doesn't have the "Open shell" option**, as shown in my previous comment. I figure I've gone astray, but can't figure it out. Chalk it up to newbie Multipass user.

Yes, your instructions work as you say (`multipass start; multipass exec ...`). I'll have to do some more homework reading the docs.

Thanks for your help.

Cheers,
QM

-------------------------

ricab | 2023-10-11 21:02:01 UTC | #9

Hey @qm64, you are right about the missing icon in the dock. I thought I recalled a similar bug report, but I couldn't find it so here's [a new one](https://github.com/canonical/multipass/issues/3251) to track that.

The "Open Shell" option is available in the Multipass icon on the top bar, not on the dock. Can you see that one?

![image|304x352](upload://sHi6HEPjE52h0NJShLyMLD9MIZt.png)

-------------------------

qm64 | 2023-10-12 17:14:57 UTC | #10

Hi @ricab,

Yes, my bad. I see that in the menu bar. Thank!

Cheers,
QM

-------------------------

backupgeek | 2023-11-12 16:21:16 UTC | #11

Hi,

New to multipass, I have successfully installed it on my M1 Mac and played around a little bit. Now a couple of questions:

1. Where do I find the actual VM files?
2. Along that same line how do I backup my VMs
3. How do I cleanup DHCP Leases that have been created for instances that I no longer have. 
4. I somehow got a primary VM but no mount. Didn’t even create that one.

Lastly what is the whole LXD, LXC topic about?

I am used to using docker but like the idea of a full VM which makes things easier with Database backup etc. or so I am told due to being able to backup the whole VM instead of having to do database dumps but I have yet to learn so much about multipass VMs that it seems a little bit daunting.

Appreciate the help and the great explanations that have already been posted.

-------------------------

ricab | 2023-11-23 10:34:43 UTC | #12

Hi @backupgeek,

[quote="backupgeek, post:11, topic:29148"]
* Where do I find the actual VM files?
[/quote]

On macOS, multipass stores its data in  `/var/root/Library/Application\ Support/multipassd` by default. You can change this with [`MULTIPASS_STORAGE`](https://multipass.run/docs/configure-multipass-storage#heading--macos).

[quote="backupgeek, post:11, topic:29148"]
* Along that same line how do I backup my VMs
[/quote]

Multipass does not currently support backing up VMs, but you can backup the folder above by regular means. 

With the next release, Multipass will support VM *snapshots*, which can be used to recover when something goes wrong with the VM itself. However, they are stored in the same place as the VM images, so not suitable as backup against disk corruption and the like.

[quote="backupgeek, post:11, topic:29148"]
* How do I cleanup DHCP Leases that have been created for instances that I no longer have.
[/quote]

In normal operation, you shouldn't have to worry about that. However, that's sometimes useful as macOS get confused with repeated or excessive leases. You can clean them up by deleting or pruning the file `/var/db/dhcpd_leases`.

[quote="backupgeek, post:11, topic:29148"]
* I somehow got a primary VM but no mount. Didn’t even create that one.
[/quote]

The [`primary` instance](https://multipass.run/docs/primary-instance) is just an implicit instance for some commands like `start`, `stop`, and `shell`, when you don't specify arguments.

Be sure to have a look at [our docs](https://multipass.run/docs) for more, but I think you'll find that it is pretty easy to learn and use :) Hope you enjoy Multipass!

-------------------------

backupgeek | 2023-12-14 04:49:59 UTC | #13

Thank you so much for the clarification! Has helped me a lot to gain a better understanding!
Multipass is really awesome!

-------------------------

tristpost | 2024-02-02 06:44:11 UTC | #14

I just tried installing Multipass on my new Macbook Pro M3 and Sonoma 14.2.1 but I am not able to even create an Ubuntu VM - it just hang for a long time and then print an timeout error message.
How can I get past this problem?????

-------------------------

tristpost | 2024-02-02 06:49:23 UTC | #15

Is there some updated guide for M3 macs?

I just tried installing multipass on my brand new Macbook Pro M3 with MacOS 14.2.1 and nothing seem to work. Trying to create/start an Ubuntu VM just hang and eventually reports an timeout....

Does multipass at all work on M3?

-------------------------

georgeliaojia | 2024-02-02 07:47:41 UTC | #16

Hi @tristpost 

It looks like it is related to https://github.com/canonical/multipass/issues/3308 . 

Can you try https://github.com/canonical/multipass/issues/3308#issuecomment-1915745772 the test package and let us know if it works for you? It worked for many other m3 macs which did not work before.

-------------------------

backupgeek | 2024-04-27 00:45:41 UTC | #17

I do have another question, I am trying to backup my multipass instance on MacOS and was hoping to follow this instruction [https://askubuntu.com/questions/1180895/import-export-vms-from-multipass](https://askubuntu.com/questions/1180895/import-export-vms-from-multipass)

The problem is even though I have snap installed on MacOS via homebrew that I get the following:
```
sudo snap save multipass
```

```
Interacting with snapd is not yet supported on darwin.
This command has been left available for documentation purposes only.
```

What does this leave me with? Any other way or any way to backup my instance?

-------------------------

townsend | 2024-04-29 20:10:22 UTC | #18

Hi @backupgeek!

The directions you see in that AskUbuntu post are for Linux only.  For macOS and running the `qemu` driver, the instances are located in `/var/root/Library/Application\ Support/multipassd/qemu/vault/instances`.

You can confirm the driver you are using by issuing `multipass get local.driver`.

-------------------------

backupgeek | 2024-05-19 01:24:08 UTC | #19

[quote="townsend, post:18, topic:29148"]
`multipass get local.driver`
[/quote]

Sorry I somehow missed your answer I did check and yes I am using ```qemu```

[quote="townsend, post:18, topic:29148"]
`/var/root/Library/Application\ Support/multipassd/qemu/vault/instances`.
[/quote]

So just backup this folder? No need for snap?

-------------------------

ricab | 2024-05-23 12:01:11 UTC | #20

Hi @backupgeek, you'd only use `snap` on Linux. On macOS, you have to use whatever mechanism applies there (e.g. Time Machine) to cover the directory that Chris mentioned.

-------------------------

backupgeek | 2024-05-24 12:07:59 UTC | #21

Got you and "Thank you" very much for explaining that. Then I know what to do now

-------------------------

