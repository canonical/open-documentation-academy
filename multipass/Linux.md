nhart | 2023-12-07 11:18:47 UTC | #1

Multipass is a flexible, powerful tool that can be used for many purposes. In its simplest form, it can be used to quickly create and destroy Ubuntu VMs (instances) on any host machine. Used to a fuller extent, Multipass is a local mini-cloud on your laptop, allowing the testing and development of multi-instance or container-based cloud applications.

This tutorial will give you an understanding of how Multipass works, and the skills you need to use its main features.


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

Multipass is available for Linux, macOs, or Windows. To install it on your OS of choice, please follow the instructions given [here](https://multipass.run/docs/how-to-guides). Note: This tutorial demonstrates use on Linux, specifically Ubuntu, but the experience on any OS should be similar.


<a href="#heading--create-and-use-a-basic-instance"><h2 id="heading--create-and-use-a-basic-instance">Create and use a basic instance</h2></a>

<!--
The easiest way to use Multipass is with the primary instance.
Linux,
The primary instance is a Multipass virtual machine that is configured to be useful for generic purposes out of the box.

The primary instance automatically mounts the $HOME directory (files in this directory are shared between the host and the instance), and it comes with Multipass's default specs: 1GB of RAM, 5GB of disk, and 1 CPU.
-->

From the application launcher, let's start Multipass. In Ubuntu, press the super key and type Multipass, or find Multipass in the Applications panel in the lower left of the desktop.

![|800x450](https://assets.ubuntu.com/v1/949aa05e-mp-linux-1.png) 

Once we've launched the application, we should see the Multipass tray icon in the upper right section of the screen:

![|688x52](https://assets.ubuntu.com/v1/5ec546da-mp-linux-2.png) 

Let's click on the icon, then on "Open Shell". 

![|286x274](https://assets.ubuntu.com/v1/3ecc5e7d-mp-linux-2a.png) 

Clicking this button does many things in the background: it creates a new virtual machine (instance), named `primary`, with 1GB of RAM, 5GB of disk, and 1 CPU; installs the most recent Ubuntu LTS release on that instance; mounts our $HOME directory in the instance; and opens a shell to the instance, announced by the command prompt `ubuntu@primary`. You can see elements of this in the printout below.

```plain
Launched: primary                                                               
Mounted '/home/<user>' into 'primary:Home'                                       
Welcome to Ubuntu 22.04.1 LTS (GNU/Linux 5.15.0-57-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Thu Jan 26 08:06:22 PST 2023

  System load:  0.0               Processes:             95
  Usage of /:   30.2% of 4.67GB   Users logged in:       0
  Memory usage: 21%               IPv4 address for ens3: 10.110.66.242
  Swap usage:   0%

 * Strictly confined Kubernetes makes edge and IoT secure. Learn how MicroK8s
   just raised the bar for easy, resilient and secure K8s cluster deployment.

   https://ubuntu.com/engage/secure-kubernetes-at-the-edge

0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

ubuntu@primary:~$
```

Let's test it out! As we just learned, the previous step automatically mounted our $HOME directory in the instance. Let's use this to share data with our instance. More concretely, let's create a new folder in our $HOME directory called Multipass_Files:

![|720x405](https://assets.ubuntu.com/v1/fbfc8304-mp-linux-3.png) 

As you can see, I've added a readme file in this shared folder. Let's check for the folder and read the file from our new instance:

```plain
ubuntu@primary:~$ cd ./Home/Multipass_Files/
ubuntu@primary:~/Home/Multipass_Files$ cat README.md 
## Shared Folder

This folder could be a great place to keep files that need to be accessed by both your host machine and Ubuntu VM!

ubuntu@primary:~/Home/Multipass_Files$
```

Congratulations, you've got your first instance! 

This instance is great for when we just need a quick Ubuntu VM, but let's say we want a more customised instance. Multipass has us covered there too!

[details = "Optional Exercises"]
Exercise 1: 
When you clicked on Open Shell just now, what happened in the background was the equivalent of the CLI commands `multipass launch –name primary`  followed by  `multipass shell`. Open a terminal and try `multipass shell` (if you didn't follow the steps above, you will have to run the `launch` command first).

Exercise 2: 
In Multipass, an instance with the name `primary` is privileged. For example, it is the default argument of multipass shell. In two terminal instances, check `multipass shell primary` and `multipass shell`. Both commands should give the same result.
[/details]

<a href="#heading--create-a-customised-instance"><h2 id="heading--create-a-customised-instance">Create a customised instance</h3></a>

Multipass has a great feature to help us get started creating customised instances. Let's open a terminal and run the command `multipass find`. This shows us a list of all of the images we can launch through Multipass currently.

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

Let's launch an instance running Ubuntu 22.10 ("Kinetic Kudu") by typing the command `multipass launch kinetic`

Now we have an instance running which has been named randomly by Multipass, in my case it is called "coherent-trumpetfish".

```plain
$ multipass launch kinetic
Launched: coherent-trumpetfish
```

We can check some basic info about our new instance by running the following:

`multipass exec coherent-trumpetfish -- lsb_release -a`

This tells multipass to execute the command `lsb_release -a` on the "coherent-trumpetfish" instance.

```plain
$ multipass exec coherent-trumpetfish -- lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.10
Release:		22.10
Codename:		kinetic
```

Perhaps after using this instance for a while, we decide what we really need is the latest LTS version of Ubuntu, with a more informative name and a little more memory and disk. We can delete the "coherent-trumpetfish" instance by running

`multipass delete coherent-trumpetfish`

Let's now launch the type of instance we're looking for by running this:

`multipass launch lts --name ltsInstance --memory 2G --disk 10G --cpus 2`

<a href="#heading--manage-instances"><h2 id="heading--manage-instances">Manage instances</h3></a>

Now let's confirm this new instance has the specs we're looking for by running `multipass info ltsInstance`

```plain
$ multipass info ltsInstance                             
Name:           ltsInstance
State:          Running
IPv4:           10.110.66.139
Release:        Ubuntu 22.04.1 LTS
Image hash:     3100a27357a0 (Ubuntu 22.04 LTS)
CPU(s):         2
Load:           1.11 0.36 0.12
Disk usage:     1.4GiB out of 9.5GiB
Memory usage:   170.4MiB out of 1.9GiB
Mounts:         --
```

We've created and deleted quite a few instances now. Let's run multipass list to see what instances we currently have.

```plain
$ multipass list                                         
Name                    State             IPv4             Image
primary                 Running           10.110.66.242    Ubuntu 22.04 LTS
coherent-trumpetfish    Deleted           --               Not Available
ltsInstance             Running           10.110.66.139    Ubuntu 22.04 LTS
```

We have two instances currently running, the primary instance and our LTS machine with customised specs. Our "coherent-trumpetfish" instance is still listed, but its state is "Deleted". We can recover this instance by running `multipass recover coherent-trumpetfish`, but for right now let's delete the instance permanently by running `multipass purge`. Running `multipass list` again confirms the instance is now permanently deleted: 

```plain
$ multipass list
Name                    State             IPv4             Image
primary                 Running           10.110.66.242    Ubuntu 22.04 LTS
ltsInstance             Running           10.110.66.139    Ubuntu 22.04 LTS
```

We've now seen a few ways to create, customise, and delete an instance. Now let's put those instances to work!

<a href="#heading--put-your-instances-to-use"><h2 id="heading--put-your-instances-to-use">Put your instances to use</h2></a>

<a href="#heading--run-a-simple-web-server"><h3 id="heading--run-a-simple-web-server">Run a simple web server</h3></a>

Let's go back to that customised LTS instance we created. Take note of its IP address revealed by `multipass list` in the previous step, then run `multipass shell ltsInstance` to open a shell in the instance.

From the shell, we can now run:

```
sudo apt update

sudo apt install apache2
```

Now, let's open a browser and type in the IP address of the instance into the address bar. We should now see the default Apache homepage.

![|720x545](https://assets.ubuntu.com/v1/e106f7f9-mp-linux-4.png) 

Just like that, we've got a web server running in a Multipass instance!

We can use this web server locally for any kind of local development or testing we like. If however, we want to access this web server from the internet (e.g. from a different computer), we need an instance that is exposed to the external network.

<a href="#heading--launch-from-a-blueprint-to-run-docker-containers"><h3 id="heading--launch-from-a-blueprint-to-run-docker-containers">Launch from a Blueprint to run Docker containers</h3></a>

Some environments require a lot of configuration and setup. Multipass Blueprints are instances with a deep level of customization. The Docker Blueprint, for example, is a pre-configured Docker environment with a Portainer container already running. We can launch an instance using the Docker Blueprint by running `multipass launch docker --name docker-dev`

Once that's finished, let's run `multipass info docker-dev` to note down the IP of the new instance.

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
Load:           1.50 2.21 1.36
Disk usage:     2.6GiB out of 38.6GiB
Memory usage:   259.7MiB out of 3.8GiB
Mounts:         --
```

Let's take the IP address starting with "10" and paste it into our browser, then add a colon and the portainer default port, 9000, like this: 10.115.5.235:9000. This will take us to the Portainer login page, where we can set a username and password.

![|720x543](https://assets.ubuntu.com/v1/75a164a1-mp-linux-5.png) 

From there, let's select a local docker environment.

![|720x601](https://assets.ubuntu.com/v1/ee3ff308-mp-linux-6.png) 

From there, we can click into the newly created "local" docker endpoint, navigate to the app templates page, and select NGINX

![|720x460](https://assets.ubuntu.com/v1/86be3eae-mp-linux-7.png) 

From the Portainer dashboard, we can see the ports that nginx has available. We can navigate to the IP address of our instance followed by that port number to see that we indeed have nginx running in a docker container inside Multipass!

![|720x465](https://assets.ubuntu.com/v1/25585a03-mp-linux-8.png) 

<a href="#heading--next-steps"><h2 id="heading--next-steps">Next steps</h2></a>

Congratulations! You now have the skills you need to use Multipass proficiently. There's more to learn about Multipass and its capabilities - check out our [how-to guides](https://multipass.run/docs/how-to-guides) for ideas and for help with your project. Our [reference page](https://multipass.run/docs/reference) contains definitions of key concepts, a complete CLI command reference, settings options and more.

Let us know what you're able to get done with Multipass!

-------------------------

mscho7969 | 2023-08-23 16:15:33 UTC | #2

It seems like Screenshots located in Google Photo are disappear. :sweat_smile: 

Same in 
- [Multipass Tutorial - Windows | Multipass documentation](https://multipass.run/docs/windows-tutorial) 
- [Multipass Tutorial - macOS | Multipass documentation](https://multipass.run/docs/mac-tutorial). 

Need to be change alternative images or remove image sections.

-------------------------

townsend | 2023-07-07 12:20:42 UTC | #3

Hi @mscho7969!

Thanks for letting us know about the missing graphics!  I'll follow up and try to figure out what happened to them.

-------------------------

sally-makin | 2023-07-07 13:54:25 UTC | #4

Thanks for taking the time to report that, @mscho7969 - we appreciate your help :) We've fixed the links now so hopefully there won't be any more problems!

-------------------------

itecompro | 2023-12-01 22:07:54 UTC | #5

Under the 'Create a customised instance' heading, there is a wrong instance name (close to the end).

Wrong: `deluxe-ghoul`
Right: `coherent-trumpetfish`

> We can delete the deluxe-ghoul instance by running

-------------------------

andreitoterman | 2023-12-07 11:49:58 UTC | #6

Good catch, @itecompro. Thanks!

-------------------------

