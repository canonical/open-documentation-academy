# Java applications

Distributing a Java application for Linux while reaching the widest possible audience is complicated. Typically, the user has to make sure the JRE/SDK version and their environment are configured correctly. When a Linux distribution changes the delivered JRE, this can be problematic for applications.

Snaps solve these problems and ensure the correct JRE is shipped alongside the application at all times.

## Why are snaps good for Java projects?

* **Snaps are easy to discover and install**
  Millions of users can browse and install snaps graphically in the Snap Store or from the command-line.
* **Snaps install and run the same across Linux**
  They bundle the exact version of whatever is required, along with all of your app's dependencies, be they Java modules or system libraries.
* **Snaps automatically update to the latest version**
  Four times a day, users' systems will check for new versions and upgrade in the background.
* **Upgrades are not disruptive**
  Because upgrades are not in-place, users can keep your app open as it's upgraded in the background.
* **Upgrades are safe**
  If your app fails to upgrade, users automatically roll back to the previous revision.

## Build a snap in 20 minutes

Ready to get started? By the end of this guide, you'll understand how to make a snap of your Java app that can be published in the [Snap Store](https://snapcraft.io/store), showcasing it to millions of Linux users.

> :information_source: For a brief overview of the snap creation process, including how to install *snapcraft* and how it's used, see [Snapcraft overview](/t/snapcraft-overview/8940). For a more comprehensive breakdown of the steps involved, take a look at [Creating a snap](/t/creating-a-snap/6799).

## Getting started

Snaps are defined in a single YAML file placed either in the root folder of your project or in a directory named `snap`. The following example shows the entire *snapcraft.yaml* file for an existing project, [Cal - The Console Calendar Generator](https://github.com/frossm/cal).

Using a few lines of yaml and the snapcraft tool, a Java application, its dependencies and the correct JRE can be packaged as a snap. Don't worry, we’ll break this down.

[details=snapcraft.yaml for Cal]

```yaml
name: fcal
version: '2.7.1'
summary: Command line calendar display
description: |
  fCal is a command line calendar utility.  It will display a
  calendar on the command line with any month/year requested.  Defaults 
  to the current year. fCal can also display local holidays. See help.

grade: stable
confinement: strict
base: core22

title: fCal
website: https://github.com/frossm/cal
issues: https://github.com/frossm/cal/issues
license: MIT

# Enable faster LZO compression
compression: lzo

# Ignore useless library warnings
lint:
  ignore:
    - library

apps:
  fcal:
    command: cal-wrapper
    plugs:
      - network

parts:
  wrapper:
    plugin: dump
    source: snap/local
    source-type: local

  library:
    plugin: maven
    source: https://github.com/frossm/library.git
    source-type: git
    source-tag: 'v2023.12.03'
    maven-parameters:
      - install

    build-packages:
      - maven
      - openjdk-11-jdk-headless

  cal:
    plugin: maven
    source: https://github.com/frossm/cal.git
    source-branch: master
    source-type: git
    after:
      - library

    build-packages:
      - maven
      - openjdk-11-jdk-headless

    stage-packages:
      - openjdk-11-jre-headless

    override-prime: |
      snapcraftctl prime
      rm -vf usr/lib/jvm/java-11-openjdk-*/lib/security/blacklisted.certs
```

[/details]

## Metadata

The `snapcraft.yaml` file starts with a small amount of human-readable metadata, which usually can be lifted from the GitHub description or project README.md. This data is used in the presentation of your app in the Snap Store.

```yaml
name: fcal
version: '2.7.1'
summary: Command line calendar display
description: |
  fCal is a command line calendar utility.  It will display a
  calendar on the command line with any month/year requested.  Defaults 
  to the current year. fCal can also display local holidays. See help.
```

## Base

The base keyword declares which _base snap_ to use with  your project.  A base snap is a special kind of snap that provides a run-time environment alongside a minimal set of libraries that are common to most applications:

```yaml
base: core22
```
As used above, [`core22`](https://snapcraft.io/core22) is the current standard base for snap building and is based on [Ubuntu 22.04 LTS](https://releases.ubuntu.com/22.04/).

See [Base snaps](/t/base-snaps/11198) for more details.

## Security model

The next section describes the level of confinement applied to your app.

```yaml
confinement: devmode
```

Snaps are containerised to ensure more predictable application behaviour and greater security. Unlike other container systems, the shape of this confinement can be changed through a set of interfaces. These are declarations that tell the system to give permission for a specific task, such as accessing a webcam or binding to a network port.

It's best to start a snap with the confinement in warning mode, rather than strictly applied. This is indicated through the `devmode` keyword. When a snap is in `devmode`, runtime confinement violations will be allowed but reported. These can be reviewed by running `journalctl -xe`.

Because `devmode` is only intended for development, snaps must be set to `strict` confinement before they can be published as "stable" in the Snap Store. Once an app is working well in `devmode`, you can review confinement violations, add appropriate interfaces, and switch to `strict` confinement.

## Apps

Apps are the commands and services exposed to end users. If your command name matches the snap `name`, users will be able to run the command directly. If the names differ, then apps are prefixed with the snap `name` (`fcal.command-name`, for example). This is to avoid conflicting with apps defined by other installed snaps.

If you don’t want your command prefixed you can request an alias for it on the [Snapcraft forum](https://forum.snapcraft.io/t/process-for-reviewing-aliases-auto-connections-and-track-requests/455). These are set up automatically when your snap is installed from the Snap Store.

```yaml
apps:
  fcal:
    command: cal-wrapper
    plugs:
      - network
```

## Parts

Parts define how to build your app. Parts can be anything: programs, libraries, or other assets needed to create and run your application. These can point to local directories, remote git repositories or other revision control systems.  
In this case we have the following sources: 
* `cal`: A remote git repository that contains the source code for the `Cal` application.
* `library`: A remote git repository that contains the source code for dependent libraries.
* `wrapper`: Path to a local directory that contains a wrapper script to run the application in a bash shell.

For details on metadata specific to snapcraft parts, see [Snapcraft parts metadata](/t/snapcraft-parts-metadata/8336).

The `maven` plugin can build the application using standard parameters. This plugin requires a `pom.xml` in the root of the source tree. For more details on Maven-specific metadata, see [The Maven plugin](/t/the-maven-plugin/4282).  

The `dump` plugin dumps the content from a specified source. For more details on metadata specific to the `dump` plugin, see [The Dump plugin](/t/the-dump-plugin/8007).



```yaml
parts:
  wrapper:
    plugin: dump
    source: snap/local
    source-type: local

  library:
    plugin: maven
    source: https://github.com/frossm/library.git
    source-type: git
    source-tag: 'v2023.12.03'
    maven-parameters:
      - install

    build-packages:
      - maven
      - openjdk-11-jdk-headless

  cal:
    plugin: maven
    source: https://github.com/frossm/cal.git
    source-branch: master
    source-type: git
    after:
      - library

    build-packages:
      - maven
      - openjdk-11-jdk-headless

    stage-packages:
      - openjdk-11-jre-headless

    override-prime: |
      snapcraftctl prime
      rm -vf usr/lib/jvm/java-11-openjdk-*/lib/security/blacklisted.certs
```

## Building the snap

You can download the example repository with the following command:

```
$ git clone https://github.com/frossm/cal.git
```

After you’ve reviewed the `snapcraft.yaml`, you can build the snap by simply executing the snapcraft command in the project directory:

```bash
$ snapcraft
```

The resulting snap can be installed locally. This requires the `--dangerous` flag because the snap is not signed by the Snap Store. The `--devmode` flag acknowledges that you are installing an unconfined application:

```bash
$ sudo snap install fcal_*.snap --devmode --dangerous
```

You can then try it out:

```bash
$ fcal
```

Removing the snap is simple too:

```bash
$ sudo snap remove fcal
```

## Publishing your snap

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/dev/account/). Here you can customise how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique “developer namespace” as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```bash
$ snapcraft login
```

## Reserve a name for your snap

You can publish your own version of a snap, provided you do so under a name you have rights to. You can register a name on [dashboard.snapcraft.io](https://dashboard.snapcraft.io/register-snap/), or by running the following command:

```bash
$ snapcraft register myjavasnap
```

Be sure to update the `name:` in your `snapcraft.yaml` to match this registered name, then run `snapcraft` again.

## Upload your snap

Use snapcraft to push the snap to the Snap Store.

```bash
$ snapcraft upload --release=edge myjavasnap_*.snap
```

If you’re happy with the result, you can commit the snapcraft.yaml to your GitHub repo. You can optionally enable [Build from GitHub](/t/build-from-github/26004) so any further commits automatically get released to edge, without requiring you to manually build locally.

Congratulations! You've just built and published your first Java snap. For a more in-depth overview of the snap building process, see [Creating a snap](/t/creating-a-snap/6799).
