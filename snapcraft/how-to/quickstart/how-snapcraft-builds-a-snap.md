# How Snapcraft builds snaps
The Snapcraft tool can perform a variety of operations, including:

* Snap creation operations like build, stage, prime, pack, etc.
* Store management operations like login, register, sign-build, etc.
* Extensions-specific operations like expand-extensions and list-extensions.

These operations translate into practical tasks like:

* Builds snaps locally or sends remote tasks to Launchpad.
* Allows the developer to register and log into the Snap Store.
* Uploads snaps to the Snap Store.
* Promotes snaps to different channels.

The list of available global options and commands can be checked with one of:

```bash
snapcraft help
snapcraft help --all
```

<h2 id='heading--snapcraft'>snapcraft.yaml</h2>

Snaps are created using a build recipe defined in a file called `snapcraft.yaml`.

When the snapcraft tool is executed on the command line, it will look for the file in the current project work directory, either in the top-level folder or a `snap` subdirectory. If the file is found, snapcraft will then parse its contents and progress the build toward completion.

`snapcraft.yaml` is a configuration file written in the YAML language, with stanzas defining the application structure and behavior. When snapcraft runs and parses this file, it will use the declared information to build the snap. For developers more familiar with the traditional Linux build systems, the process is somewhat similar to the use of a Makefile or an RPM spec file.

You can create the `snapcraft.yaml` file manually, or you can run the `snapcraft init` command to create a template file in the `snap` subdirectory.

```bash
snapcraft init
```

A generated template file contains just enough data to build, split across three stanzas:

* Metadata.
* Confinement level.
* Build definition.

<h3 id='heading--definitions'>Main definitions inside snapcraft.yaml</h3>

There is no one way for how a snap ought to be assembled. However, most `snapcraft.yaml` files have the same common elements, including a number of mandatory declarations. To learn more about the supported keys, including which ones one are mandatory or optional, please refer to the [Snapcraft.yaml schema](/t/snapcraft-yaml-schema/4276).

Below is a short list of these keys, which will be further explained in the Examples sections later in the tutorial:
* **Metadata** - describes the snap functionality and provides identifiers by which the snap can be cataloged and searched in the Snap Store.
* **Security confinement** - describes the level of security of the snap.
* **Base** - describes which set of libraries the snap will use for its functionality. The base also defines the operating system version for the snap build instance in the virtual machine or container launched by Snapcraft. For instance, base: core18 means that Snapcraft will launch an Ubuntu 18.04 virtual machine or container, the set of tools and libraries used inside the snap will originate from the Ubuntu 18.04 repository archives, and the snap applications will “think” they are running on top of an Ubuntu 18.04 system, regardless of what the actual underlying Linux distribution is.
* **Parts** - describes the components that will be used to assemble the snap.
* **Apps** - describes the applications and their commands that can be run in an installed snap.

It is important to note several additional details:

* A snap may contain one or more parts.
* A snap may contain none, one or more applications
* Parts can be pre-assembled binaries or they may be compiled as part of the build process.
* The parts section of the `snapcraft.yaml` file uses Snapcraft build system or language-specific plugins to simplify the build process.
* The parts section may also include a list of [build packages](/t/build-and-staging-dependencies/11451) (build-packages) that will be used to create the snap applications but will not be included in the final snap. For instance, gcc or make.
* The parts section may also include a list of [stage packages](/t/build-and-staging-dependencies/11451) (stage-packages) that will be used by the snap’s applications at runtime, e.g.: python-bcrypt. These will be obtained from the repository archives in the build instance.
* If you need to pull packages from different *apt* sources, see [Snapcraft package repositories](/t/snapcraft-package-repositories/15475).

<h2 id='heading--output'>Snapcraft build output</h2>

See <!--TO DO: Path to be added when the page is created--> [Parts lifecycle]() to consult the Parts lifecycle steps.

The artifact of a successful Snapcraft build run is a snap file, which is itself a compressed Squashfs archive distinguished by the .snap suffix.

A snap may contain one or more files that allow the applications to run without reliance on the underlying host system’s libraries. A snap will contain one or more applications, daemons, configuration files, assets like icons, and other objects.

Typically, the content of a snap will resemble a Linux filesystem layout:

```no-highlight
drwxr-xr-x 10 igor igor  4096 Jun 10  2020 ./
drwxrwxrwx 14 igor igor 16384 Oct 17 16:40 ../
drwxr-xr-x  2 igor igor  4096 Jun 10  2020 bin/
drwxr-xr-x 10 igor igor  4096 Jun 10  2020 etc/
-rw-r--r--  1 igor igor	14 Jun 10  2020 flavor-select
drwxr-xr-x  3 igor igor  4096 Jun 10  2020 lib/
drwxr-xr-x  2 igor igor  4096 Jun 10  2020 lib64/
drwxr-xr-x  3 igor igor  4096 Jun 10  2020 meta/
drwxr-xr-x  3 igor igor  4096 Jun 10  2020 snap/
drwxr-xr-x  7 igor igor  4096 Jun 10  2020 usr/
drwxr-xr-x  3 igor igor  4096 Feb 26  2018 var/
```

The end user can examine the contents of a snap by either looking through the 'prime' directory of the build environment, or by extracting the snap archive:

```bash
unsquashfs <file>.snap
```
