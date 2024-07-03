In this tutorial you will build a snap package for a Python application called [liquitctl](https://github.com/liquidctl/liquidctl) using Snapcraft, which is the build ecosystem for creating, publishing and maintaining snaps.

The concepts covered in this tutorial are applicable to all snaps, regardless of their complexity. We'll cover everything from creating the build environment and the configuration file, to troubleshooting missing libraries and which interfaces may be required.

- [Requirements](#heading--requirements)
- 1\. [Snapcraft setup](#heading--setup)
  * 1.1 [Snapcraft build environment](#heading--build-environment)
  * 1.2 [Create a YAML template](#heading--yaml)
  * 1.3 [Build a template snap](#heading--build-template)    
- 2\. [Modify the snapcraft.yaml](#heading--modify)
  * 2.1 [Create a new part](#heading--part)
  * 2.2 [Build the part](#heading--build-part)
- 3\. [Create an app section](#heading--expose)
  * 3.1 [Install the snap in developer mode](#heading--developer-mode)
- 4\. [Test the snap](#heading--test)
  * 4.1 [Missing dependencies](#heading--missing)
  * 4.2 [System access](#heading--system-access)
- 5\. [Update the confinement level](#heading--confinement)

<a id='heading--requirements'></a>

### Requirements

Snapcraft can be installed on various Linux distributions, as well as on macOS and Windows operating systems. For this tutorial, however, we recommend using [Ubuntu 22.04 LTS (Jammy Jellyfish)](https://releases.ubuntu.com/jammy/) or later.

This tutorial does not require any programming or specific Linux knowledge, but you will need some familiarity with the Linux command line. All the instructions are run as commands from the [Terminal](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) application.

Your system also needs to have at least 20GB of storage available.

<a id='heading--setup'></a>

## 1. Snapcraft setup

From the terminal, type the following to install Snapcraft:

```bash
sudo snap install snapcraft --classic
```

<h3 id='heading--build-environment'>1.1 Snapcraft build environment</h3>

Snapcraft builds snaps within an [LXD](https://ubuntu.com/lxd) container environment by default. This keeps a snap build isolated from your system and ensures that any dependencies the snap requires are only provided by the build process.

To install LXD, type the following:

```bash
sudo snap install lxd
```

You also need to add your current user to the `lxd` group to give yourself permission to access its resources:

```bash
sudo usermod -a -G lxd $USER
```

Logout and re-open your user session for the new group to become active.

LXD can now be initialised with the 'lxd init' command:

```bash
lxd init --minimal
```

See [How to install LXD](https://documentation.ubuntu.com/lxd/en/latest/installing/#installing) for further installation options and troubleshooting.

<h3 id='heading--yaml'>1.2 Create a YAML template</h3>

Start by creating a new directory to hold the snap data, and then `cd` into this directory:

```bash
mkdir mysnap
cd mysnap
```

To create a new YAML template for a working snap, run `snapcraft init` within this directory:

```
snapcraft init
```

The YAML template file is called `snapcraft.yaml` and you'll find it within a new `snap` sub-directory.

<h3 id='heading--build-template'>1.3 Build a template snap</h3>

The template file contains enough information to build a snap without any further modifications. This can be accomplished by running the `snapcraft` command in the parent directory:

```
snapcraft
```

In the background, Snapcraft will create a new LXD container, install into this whatever the template file contains, and build a snap. The output will look similar to the following and the resultant snap can be found in the current directory:

```bash
Launching instance...
Executed: pull my-part
Executed: build my-part
Executed: stage my-part
Executed: prime my-part
Executed parts lifecycle
Generated snap metadata
Created snap package my-snap-name_0.1_amd64.snap
```

<h2 id='heading--modify'>2. Modify the snapcraft.yaml</h2>

The `snap/snapcraft.yaml` file describes the application, its dependencies and how it should be built. It currently contains the following metadata:

```yaml
name: my-snap-name # you probably want to 'snapcraft register <name>'
base: core22 # the base snap is the execution environment for this snap
version: '0.1' # just for humans, typically '1.2+git' or '1.3.2'
summary: Single-line elevator pitch for your amazing snap # 79 char long summary
description: |
  This is my-snap's description. You have a paragraph or two to tell the
  most important story about your snap. Keep it under 100 words though,
  we live in tweetspace and your description wants to look good in the snap
  store.

grade: devel # must be 'stable' to release into candidate/stable channels
confinement: devmode # use 'strict' once you have the right plugs and slots

parts:
  my-part:
    # See 'snapcraft plugins'
    plugin: nil
```

The above metadata is enough to build a snap, but the snap has no functionality. To create a functional snap, we need expand the `parts:` section and add a new section called  `app:`.

<h3 id='heading--part'>2.1 Create a new part</h2>

A snap is assembled from one or more _parts_ and each part describes a component required for the snap to function. This component could be a library or an executable, for example, and parts use _plugins_ to construct and organise whatever components are needed.

Our application is built with Python, and Snapcraft includes a [Python plugin](/t/the-python-plugin/8529) to automatically handle its dependencies and install requirements.

Open `snap/snapcraft.yaml` with your favourite text editor and navigate to the bottom line, `plugin: nil`. Replace `nil` with `python` and add the following `source-type` and `source` lines:

```yaml
    plugin: python
    source-type: git
    source: https://github.com/liquidctl/liquidctl
```

This is all that is required for Snapcraft to access, clone locally, and build the upstream source code of the project.

Running `snapcraft` again would build the application and create a new snap. However, this new snap would still not function because we have not yet told Snapcraft which executable to expose and run.

<h3 id='heading--build-part'>2.2 Build the part</h3>

A snap is built in several stages, collectively known as the _parts lifecycle_, as shown in Snapcraft's build output.

1. **Pull** retrieves whatever is required for each part to be built
1. **Build** constructs each part using each respective plugins
1. **Stage** copies built components into a shared staging area
1. **Prime** moves staged files and directories into their final locations

This is important because you can stop a build at any stage to look inside the build container.

Run the following `snapcraft` command to both start a new snap build and run the build up to the _prime_ step. The command will also open a shell within the build environment.

```bash
snapcraft prime --shell
```

> :information_source: If you've already built the same snap, run `snapcraft clean` first to reset the build environment.


From the build shell prompt inside the container, type `cd $HOME` to change to Snapcraft's build directory, and `ls` to see its contents:

```
environment.sh  parts  prime  project  snap  stage
```

These directories hold the data for each build stage, while the `environments.sh` file contains the environment variable configuration.

The executable name is `liquidctl`, which we can now search for:

```bash
$ find . -name liquidctl
./project/squashfs-root/bin/liquidctl
./parts/my-part/build/build/lib/liquidctl
./parts/my-part/build/liquidctl
./parts/my-part/src/liquidctl
./parts/my-part/install/bin/liquidctl
./parts/my-part/install/lib/python3.10/site-packages/liquidctl
./stage/bin/liquidctl
./stage/lib/python3.10/site-packages/liquidctl
```

The above output shows how the Python plugin has built and installed the executable within the container. The final binary is in `./stage/bin`.

Type `exit` to quit the build environment shell.

<a id='heading--expose'></a>

## 3. Create an app section

To use the binary's location and permit access, you need to declare it within the `apps` section of your `snapcraft.yaml` file:

```yaml
apps:
  my-snap-name:
    command: bin/liquidctl
```

If the sub-section name matches the snap name it becomes the default executable for the snap.

This means that when our snap is installed, typing `my-snap-name` will run the `bin/liquidctl` binary . It's more usual for a snap name to match the name of the executable.

<h3 id='heading--developer-mode'>3.1 Install the snap in developer mode</h3>

The snap can now be rebuilt to produce what should be an installable and executable snap package.

First, ensure that you have successfully exited the build environment shell by running the `exit` command. Then run the command, `snapcraft`. This will produce a snap package called `my-snap-name_0.1_amd64.snap` (depending on your system architecture).

```bash
Created snap package my-snap-name_0.1_amd64.snap
```

This snap package can be installed locally with the snap command, invoking both `--devmode` and `--dangerous` options to permit system access and installation without verification:

```bash
sudo snap install ./my-snap-name_0.1_amd64.snap --dangerous --devmode
```

With the snap installed, the `my-snap-name` command can now be run to execute `liquidctl`:

```bash
$ my-snap-name
Usage:
  liquidctl [options] list
  liquidctl [options] initialize [all]
[...]
```

<a id='heading--test'></a>

## 4. Test the snap

To test a snap properly, it needs to be run as intended. The `liquidctl` command, for example, accesses USB devices to read and set proprietary sensor, fan and LEDs values.

Even without such devices connected, the `list` will attempt to discover any connected devices:

```bash
$ my-snap-name list
usb.core.NoBackendError: No backend available
```

This produces an error, and unless you know the project code, it's difficult to say from the error whether it's a problem with the snap, a problem with not having the hardware, or a problem with our test system.

<h3 id='heading--missing'>4.1 Missing dependencies</h3>

If you're a Python developer, or reasonably good at searching the internet, it's relatively straightforward to work out that the `usb.core.NoBackendError` issue is caused by a missing `python3-usb` package. This can be added through a new `stage-packages` section for the part. Stage packages are those packages you wish to be installed alongside the application:

```yaml
parts:
  my-part:
    plugin: python
    source-type: git
    source: https://github.com/liquidctl/liquidctl
    stage-packages:
       - python3-usb
```

Now reset your build environment by running `snapcraft clean`. After resetting your build environment, rebuild it with the command, `snapcraft`. Then install the new snap by running:

```bash
sudo snap install ./my-snap-name_0.1_amd64.snap --dangerous --devmode
```

If you have any supported devices, running the command `my-snap-name list` should now give you an output similar to the following:

```bash
 Device #0: Corsair HX750i
 Device #1: Corsair Hydro H100i v2
```

See [Supported devices](https://github.com/liquidctl/liquidctl#supported-devices) for the list of supported devices.

<h3 id='heading--system-access'>4.2 System access</h3>

Running our snap with real hardware will result in an insufficient permissions error and this is because snaps limit system access by default. [Interfaces](/t/supported-interfaces/7744) are used to permit access to individual resources through _plugs_ and _slots_.

Plugs declares which [interfaces](/t/supported-interfaces/7744) an app needs to function, such as [home](/t/the-home-interface/7838) to access local files, or [network](/t/the-network-interface/7880) to access the network. In this case, _liquidctl_ needs access to USB devices, which can be satisfied with the [raw-usb](/t/the-raw-usb-interface/7908) interface for device input and output, [uhid](/t/the-uhid-interface/7931) for user access, and [hardware-observe](/t/the-hardware-observe-interface/7833) to enable the system to see which devices are connected.

These can added with the creation of a new `plugs:` sections beneath the command name for the app:

```
apps:
  my-snap-name:
    command: bin/liquidctl
    plugs:
      - raw-usb
      - uhid
      - hardware-observe
```

When the snap is installed, the interfaces are can be activated manually with the following commands:


```bash
sudo snap connect my-snap-name:uhid
sudo snap connect my-snap-name:raw-usb
sudo snap connect my-snap-name:hardware-observe
```

The snap can now be run without encountering any further errors or missing functionality.

<a id='heading--confinement'></a>

## 5. Update confinement level

The final step when building any snap is to change its grade to `stable` and its confinement to `strict`. Both of these values are at the top of the snapcraft.yaml file and they default to developer-friendly options so that errors only report themselves rather than stop functionality. They're useful when building a snap but are far less secure when you want to share it.

```yaml
grade: stable # must be 'stable' to release into candidate/stable channels
confinement: strict # use 'strict' once you have the right plugs and slots
```

The snap is now fully functional and can be rebuilt and installed. At this point, your own snaps could be published and shared.
