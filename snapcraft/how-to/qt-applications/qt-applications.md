Snapcraft can be used to package and distribute Qt 5 applications, including applications that use the KDE Frameworks 5 add-on libraries, in a way that enables convenient installation by users.

The process of creating a snap for a Qt 5 and KDE Frameworks 5 application builds on standard tools like `cmake`, `make` and `qmake`. This enables developers to re-use the application's existing build system when creating a snap, making packaging easy.

[note]

For a brief overview of the snap creation process, including how to install Snapcraft and how it's used, see [Snapcraft overview](/t/snapcraft-overview/8940). For a more comprehensive breakdown of the steps involved, take a look at [Creating a snap](/t/creating-a-snap/6799).
[/note]

## Creating a new Qt 5 and KDE Frameworks 5 based snap

Snaps are defined in a single YAML file named *snapcraft.yaml* located in the root folder of your project. This YAML file describes the application, its dependencies and how it should be built.

For this how-to, we will create and build a snap of KDE's calculator application [KCalc](https://apps.kde.org/kcalc/). We will also explore how we can submit our snap to the Snap Store.

We're going to start by creating a *snapcraft.yaml* file that contains the minimum needed to build a working version of KCalc. We'll explain the contents of this file line-by-line in the following sections of this guide.

[details=Our minimal snapcraft.yaml for KCalc]
```yaml
name: kcalc-example
adopt-info: build-kcalc
grade: devel

base: core22
confinement: devmode

apps:
  kcalc-example:
    common-id: org.kde.kcalc.desktop
    command: usr/bin/kcalc
    extensions:
      - kde-neon

parts:
  build-kcalc:
    source: https://invent.kde.org/utilities/kcalc/-/archive/v23.08.5/kcalc-v23.08.5.tar.gz
    parse-info:
      - usr/share/metainfo/org.kde.kcalc.appdata.xml
    build-packages:
      - libmpfr-dev
      - libgmp-dev
    stage-packages:
      - libmpfr6
      - libgmp10
    plugin: cmake
    cmake-parameters:
      - "-DCMAKE_BUILD_TYPE=Release"
      - "-DCMAKE_INSTALL_PREFIX=/usr"
      - "-DKDE_INSTALL_USE_QT_SYS_PATHS=ON"
      - "-DKDE_SKIP_TEST_SETTINGS=ON"
      - "-DINSTALL_ICONS=ON"
      - "-DKF5DocTools_FOUND=OFF"
```
[/details]

### Metadata

The *snapcraft.yaml* file starts with a small amount of human-readable metadata that determines how the snap is presented in the Snap Store. See [Snapcraft top-level metadata](/t/snapcraft-top-level-metadata/8334) for more information.

```yaml
name: kcalc-example
adopt-info: build-kcalc
grade: devel
```

A good place to start is by giving the snap a `name`. We're naming our snap `kcalc-example`. Valid names consist of up to 40 lower-case ASCII letters, numbers and hyphens. Names must contain at least one letter, and they cannot start or end with a hyphen. If you want to publish your snap, you'll need to specify a `name` that hasn't already been taken in the Snap Store.

It's also necessary to:
1. include a short `summary` of what the snap does, a longer `description`, and the `version` number of the snap; or
2. use the `adopt-info` attribute to tell Snapcraft that this information will be defined during the build process by the named build part (for example, using a shell script or an [AppStream metadata file](/t/using-external-metadata/4642))
We're opting to use `adopt-info` for our snap, as the developers of KCalc have bundled a suitable AppStream file with the application's source code which contains most of the metadata needed by Snapcraft.

The `grade` is optional. It defines the quality of the snap. We're setting the `grade` as `devel` to tell Snapcraft that our snap is in development, and that it isn't ready to be published to the `stable` or `candidate` release [channels](/t/channels/551) of the Snap Store.

### Base

The `base` keyword defines a special kind of snap that provides a run-time environment with a minimal set of libraries that are common to most applications. Bases are transparent to users, but they need to be considered, and specified, when building a snap. See [Base snaps](/t/base-snaps/11198) for more information.

```yaml
base: core22
```

[`core22`](https://snapcraft.io/core22) is a standard base for snap building and is based on [Ubuntu 22.04 LTS](https://releases.ubuntu.com/22.04/). In addition, it offers the best experience for building Qt 5 and KDE Frameworks 5 applications via the `kde-neon` extension, which we'll cover later on.

### Security model

The `confinement` defines the extent to which your snap is [isolated](/t/snap-confinement/6233) from user systems.

```yaml
confinement: devmode
```

It's worth setting the `confinement` to `devmode` when you start working on a new snap. `devmode` causes the snapped application to be run *without* the security confinement imposed by the [snap daemon](/t/installing-snapd/6735). It can also help you to identify the [interfaces](/t/supported-interfaces/7744) that your snap needs to access, speeding up the snap building process. See [Debug snaps](/t/debugging-snaps/18420) for more information.

Once your snap is ready, you'll need to change the `confinement` to `strict` and test that the snap still works, before you make it available generally to users of the Snap Store. (`devmode` snaps may only be released to the hidden "edge" channel of the Snap Store where you and other developers/testers can install and test them.)

### Apps

Apps are the commands or background services that you want your users to run. They are defined in the `apps` section of *snapcraft.yaml*. See [Snapcraft app and service metadata](/t/snapcraft-app-and-service-metadata/8335) for more information.

A single *snap* can contain multiple *apps*. That said, we only need to define one app for our snap, which we're naming `kcalc-example`:

```yaml
apps:
  kcalc-example:
    common-id: org.kde.kcalc.desktop
    command: usr/bin/kcalc
    extensions:
      - kde-neon
```

The name that you give each *app* matters, as it determines the command that needs to be run to launch the *app* from a terminal. See [Commands and aliases](/t/commands-and-aliases/3950) for more information.

If you're relying on an AppStream metadata file, add a `common-id` attribute to each `apps` entry and set the value to the AppStream *component identifier* for that app. This enables Snapcraft to locate app-specific metadata such as the location of a [desktop entry file](/t/desktop-menu-support/13115) from the metadata file. See [Using AppStream metadata](/t/using-external-metadata/4642#heading--appstream) for more information. (Alternatively, if you're not using an AppStream metadata file, you can still specify the location of a [desktop entry file](/t/desktop-menu-support/13115) by adding a `desktop` attribute to each `apps` entry.)

The `command` is the path to the app's main executable file within the snap. 

The [`kde-neon` extension](/t/the-kde-neon-extension/13752) ensures that the relevant Qt/KDE libraries are available during the build process and at run time, through the use of external [content snaps](/t/the-content-interface/1074). The extension also:
- adds `plugs` to the `desktop`, `desktop-legacy`, `opengl`, `wayland` and `x11` interfaces; and
- configures the run-time environment of the app, ensuring that all relevant desktop functionality is correctly initialised

If you include multiple Qt 5 and KDE Frameworks 5 *apps* in your *snap*, make sure to add the `kde-neon` extension to each *app* entry.

### Parts

The `parts` section tells Snapcraft how to build the snap. We only need to define one part for now, which we're naming `build-kcalc`:

```yaml
parts:
  build-kcalc:
    source: https://invent.kde.org/utilities/kcalc/-/archive/v23.08.5/kcalc-v23.08.5.tar.gz
    parse-info:
      - usr/share/metainfo/org.kde.kcalc.appdata.xml
    build-packages:
      - libmpfr-dev
      - libgmp-dev
    stage-packages:
      - libmpfr6
      - libgmp10
    plugin: cmake
    cmake-parameters:
      - "-DCMAKE_BUILD_TYPE=Release"
      - "-DCMAKE_INSTALL_PREFIX=/usr"
      - "-DKDE_INSTALL_USE_QT_SYS_PATHS=ON"
      - "-DKDE_SKIP_TEST_SETTINGS=ON"
      - "-DINSTALL_ICONS=ON"
      - "-DKF5DocTools_FOUND=OFF"
```

`source` tells Snapcraft where to find the source material for a part. We've specified the URL of a compressed tarball file hosted on a web server, which Snapcraft can download and unpack automatically. Snapcraft can also pull from version control systems like Git or SVN, or use a local directory or archive file. See [Snapcraft parts metadata](/t/snapcraft-parts-metadata/8336) for more information.

If you're using an [AppStream metadata file](/t/using-external-metadata/4642#heading--appstream), then set `parse-info` to the location of that file in your snap. Snapcraft will parse this file and extract:
1. global metadata for your snap, such as the `summary` and `description`, if *snapcraft.yaml* includes an `adopt-info` attribute that names this part; and
2. per-app metadata, such as the location of the relevant desktop entry file, where the app's `common-id` matches a component ID in the AppStream file

In addition to Qt 5 and KDE Frameworks 5, KCalc depends on the MPFR and GMP calculation libraries. We use:
- `build-packages` to ensure that the *development* versions of these libraries are during the build process; and
- `stage-packages` to bundle the *run-time* versions within the snap
As we're using `core22` as the `base` for our snap, and as we haven't defined any additional software repositories in *snapcraft.yaml*, Snapcraft will obtain these packages from the *Ubuntu 22.04 LTS* [package archive](https://packages.ubuntu.com). There is no need to list any of the Qt or KDE libraries here, as the `kde-neon` extension makes them available automatically.

We set the `plugin` to [`cmake`](/t/the-cmake-plugin/8621) to tell Snapcraft to build this part using the *CMake build system*.

We can pass various build-modifying parameters to CMake using `cmake-parameters`. For example:
- `"-DINSTALL_ICONS=ON"` embeds KCalc's icons within the snap, so that they correctly appear in the user's application launcher. If we omitted this parameter, then the icon that users would see would depend on their desktop environment and configuration.
- `"-DKF5DocTools_FOUND=OFF"` disables the generation of KCalc's documentation files. This helps to speed up the build process and reduce the size of our snap. Users can still access the documentation online via KCalc's *Help* menu.

## Building the snap

You can download the repository for our `kcalc-example` snap by running the following command:

```bash
git clone https://github.com/snapcraft-docs/kcalc-example
```

The *snapcraft.yaml* file that we've discussed so far is located in the *initial* sub-directory.

You can build the snap by running `snapcraft` in that directory. The build process should take about 5 to 10 minutes to complete. Snapcraft will display various status messages in your terminal whilst the build is ongoing. If all goes well, you should eventually see something similar to the following:

```bash
$ snapcraft
Generated snap metadata
Created snap package kcalc-example_23.08.5_amd64.snap
```

[note]

Although we have only tested this example on an *amd64* system, the `kde-neon` extension is also available for *arm64*.
[/note]

The resulting snap can be installed locally. You need to specify the `--devmode` option to install a snap that uses `devmode` confinement, for example:

```bash
$ sudo snap install kcalc-example_*.snap --devmode
```

You can then try it out by running one of the following commands:

```bash
$ snap run kcalc-example
```
```bash
$ kcalc-example
```

Removing the snap is simple too:

```bash
$ sudo snap remove kcalc-example
```

You can clean up the build environment with the following command:

```bash
$ snapcraft clean
```

By default, when you make a change to *snapcraft.yaml*, Snapcraft only builds the parts that have changed. Cleaning a build, however, forces your snap to be rebuilt in a clean environment and will take longer.

## Improving the snap

You might notice a couple of issues if you test our KCalc snap, namely:
- the application might use different cursors to the rest of your applications
- error sounds might not play

Let's try to improve the user experience by addressing them:

[details=Setting cursor themes]
Qt 5 applications that run in a [Wayland environment](https://wayland.freedesktop.org) expect to learn about the user's cursor theme and size from the environment variables `XCURSOR_THEME` and `XCURSOR_SIZE`. However, not all desktop environments set these variables, causing Qt 5 to fall back on a set of built-in defaults.

We can try to resolve this issue by adding a shell script to our snap that attempts to set `XCURSOR_THEME` and `XCURSOR_SIZE` whenever the user launches our app.

We can do this by adding a new part named `set-cursor-variables`:

```yaml
  set-cursor-variables:
    source: https://github.com/snapcraft-docs/kcalc-example.git
    source-type: git
    source-subdir: scripts
    plugin: dump
    organize:
      set-cursor-variables: snap/command-chain/set-cursor-variables
```

The various `source` lines tell Snapcraft to download our `kcalc-example` GitHub repository and change to the `scripts` directory. This directory contains a shell script named [`set-cursor-variables`](https://github.com/snapcraft-docs/kcalc-example/blob/scripts/set-cursor-variables) which attempts to query the user's settings using [D-Bus](https://www.freedesktop.org/wiki/Software/dbus/) and set the two values accordingly. The `dump` `plugin` tells Snapcraft that we want to download the GitHub repository without running any build tools like `make` or `cmake`. The `organize` attribute copies the script to a directory in our snap.

We also need to add a `command-chain` entry to the `kcalc-example` app definition, with the value set to the location of the script within the snap. This entry causes the script to be run whenever the app is launched. Once updated, the `apps` section should read:

```yaml
apps:
  kcalc-example:
    common-id: org.kde.kcalc.desktop
    command: usr/bin/kcalc
    command-chain:
      - snap/command-chain/set-cursor-variables
    extensions:
      - kde-neon
```

[note]

The snap confinement system prevents the app from accessing the host system's cursor store. This means that the user's chosen theme needs to be contained within the `gtk-common-themes` or Qt/KDE Frameworks content snaps. If it isn't, then the application will fall back to the Qt 5 provided default theme.
[/note]
[/details]

[details=Playing error sounds]
There are several situations where KCalc will attempt to play an error sound, such as when a user tries to add multiple decimal points to a number. However, it fails to play these sounds by default.

To fix this, we first need to connect the app to the `audio-playback` interface, by adding a `plugs` entry with the value `audio-playback` to the `kcalc-example` app definition. The resulting `apps` section will look like this:

```yaml
apps:
  kcalc-example:
    common-id: org.kde.kcalc.desktop
    command: usr/bin/kcalc
    extensions:
      - kde-neon
    plugs:
      - audio-playback
```

(If you followed the 'Cursor themes' part of this guide, the resulting `apps` section will also include a `command-chain` entry.)

The `audio-playback` interface enables an app to play sounds whilst under `strict` confinement. It isn't necessary when testing a snap under `devmode` confinement, as `devmode` allows snaps to access any interface it needs, so if you're having sound-related issues in `devmode` it's likely that there is another issue (such as a missing library, which is also the case here).

KCalc plays error sounds using KDE's *KNotification* Framework, which in turn relies upon the *libcanberra* audio system. The `kde-neon` extension provides the main *libcanberra* library, but not the required *libcanberra-pulse* plugin. 

As the plugin is contained in the `libcanberra-pulse` package in the `core22` software archive, we *could* use `stage-packages` to download and install the package, in the same way we did for the MPFR and GMP libraries earlier on. Snapcraft would then fetch that package for us, as well as any other library or data packages that it depends on, at the start of the build.

However, we don't need Snapcraft to download each of the dependencies of *libcanberra-pulse* as they're already contained in the Qt/KDE content snap. Instead, to keep the size of our snap as small as possible, we can use `override-pull` and `override-build` to tell Snapcraft how to download and extract just the additional package that we're interested in.

We can do this by adding a new part to *snapcraft.yaml* named `sound-support` as follows:

```yaml
  sound-support:
    plugin: nil
    override-pull: |
      if [ ! -e libcanberra-pulse_*.deb ]; then
        apt-get download libcanberra-pulse:$CRAFT_ARCH_BUILD_FOR
      fi
    override-build: |
      if [ ! -e $CRAFT_PART_INSTALL/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/libcanberra-*/libcanberra-pulse.so ]; then
        dpkg -x $CRAFT_PART_SRC/libcanberra-pulse_*.deb $CRAFT_PART_INSTALL
      fi
```

This part defines custom *pull* and *build* steps as follows:
- `override-pull` runs *apt-get* to download the *libcanberra-pulse* package from the `core22` package archives
- `override-build` runs *dpkg* to extract the contents of the downloaded package to the `sound-support` part's installation directory, ready to be bundled in the snap

We set the `plugin` to `nil` as we don't want Snapcraft to take any actions other than those listed in our custom *stage* and *prime* steps.

--

Finally, KCalc expects to find the library file (*libcanberra-pulse.so*) and the sound effect (*Oxygen-Sys-App-Message.ogg*) in particular locations in the `/usr` directory, rather than the actual locations of those files in our snap. We can add a `layout` section to create links between the *expected* and *actual* locations:

```yaml
layout:
  /usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/libcanberra-0.30/libcanberra-pulse.so:
    symlink: $SNAP/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/libcanberra-0.30/libcanberra-pulse.so
  /usr/share/sounds/Oxygen-Sys-App-Message.ogg:
    symlink: $SNAP/kf5/usr/share/sounds/freedesktop/stereo/dialog-information.oga
```

See [Snap layouts](/t/snap-layouts/7207) for more information.

If you make these changes to *snapcraft.yaml*, and then re-build and reinstall the snap, you should now hear a sound when an error occurs.
[/details]

Other changes that are worth making include:

[details=Completing additional fields]
There are various optional [metadata attributes](/t/snapcraft-top-level-metadata/8334) that can be added to *snapcraft.yaml* to help prospective and existing users learn more about the snap, such as:
1. the [SPDX identifier](https://spdx.org/licenses/) of the copyright `license` that applies to the snapped applications
1. a `contact` URL or email address
1. a link to the main `website` for the snap
1. a link to a website where users can report any `issues` with the snap (for example, a GitHub/GitLab issues page, or a Discourse forum)
1. a link to the official `source-code` repository for the snap, where the *snapcraft.yaml* file can be found
1. the `title` of the snap, to be displayed in store frontends such as GNOME Software and KDE Discover
1. the location of the `icon` that users should see in the Snap Store and in store frontends

Snapcraft will obtain the `title` and `icon` from an AppStream metadata file if the `adopt-info` and `parse-info` attributes have been correctly defined.

We can add these metadata fields to our snap as follows:

```yaml
license: GPL-2.0-or-later
website: https://github.com/snapcraft-docs/kcalc
contact: https://github.com/snapcraft-docs/kcalc
issues: https://github.com/snapcraft-docs/kcalc/issues
source-code: https://github.com/snapcraft-docs/kcalc
```
[/details]

[details=Updating the confinement to `strict`]
Once you have a working snap, you should update the `confinement` from `devmode` to `strict` and carry out further rounds of testing, to ensure that the snap has access to each of the required interfaces:

```yaml
confinement: strict
```

To install a snap that uses `strict` confinement, use the `--dangerous` flag rather than `--devmode`:

```bash
$ sudo snap install kcalc-example_*.snap --dangerous
```

The `--dangerous` flag is only needed for `strict` confinement snaps that *haven't* been signed and released via the Snap Store, such as the snaps that you'll generate during the development process.

[note]
Avoid using the `--devmode` flag for `strict` confinement snaps
The `--devmode` flag causes a snap to be installed using `devmode` confinement with full access to snap interfaces, **even** if the *snapcraft.yaml* file specifies `strict` confinement and a limited set of interfaces. It's worth double checking that you're using the `--dangerous` flag when you're installing and testing a `strict` confinement snap, otherwise you may find it difficult to test whether the snap is working as intended. 
[/note]
[/details]

### The updated snap

Here's the updated *snapcraft.yaml* for our snap. If you cloned the *kcalc-example* repository from GitHub earlier on, you should find a copy of this file in the *revised* folder of that repository.

[details=The updated version of our snapcraft.yaml for KCalc]
```
name: kcalc-example
adopt-info: build-kcalc
grade: devel

license: GPL-2.0-or-later
website: https://github.com/snapcraft-docs/kcalc-example/
contact: https://github.com/snapcraft-docs/kcalc-example/
issues: https://github.com/snapcraft-docs/kcalc-example/issues/
source-code: https://github.com/snapcraft-docs/kcalc-example/

base: core22
confinement: strict

apps:
  kcalc-example:
    common-id: org.kde.kcalc.desktop
    command: usr/bin/kcalc
    command-chain:
      - snap/command-chain/set-cursor-variables
    extensions:
      - kde-neon
    plugs:
      - audio-playback

parts:
  build-kcalc:
    source: https://invent.kde.org/utilities/kcalc/-/archive/v23.08.5/kcalc-v23.08.5.tar.gz
    parse-info:
      - usr/share/metainfo/org.kde.kcalc.appdata.xml
    build-packages:
      - libmpfr-dev
      - libgmp-dev
    stage-packages:
      - libmpfr6
      - libgmp10
    plugin: cmake
    cmake-parameters:
      - "-DCMAKE_BUILD_TYPE=Release"
      - "-DCMAKE_INSTALL_PREFIX=/usr"
      - "-DKDE_INSTALL_USE_QT_SYS_PATHS=ON"
      - "-DKDE_SKIP_TEST_SETTINGS=ON"
      - "-DINSTALL_ICONS=ON"
      - "-DKF5DocTools_FOUND=OFF"

  set-cursor-variables:
    source: https://github.com/snapcraft-docs/kcalc-example.git
    source-type: git
    source-subdir: scripts
    plugin: dump
    organize:
      set-cursor-variables: snap/command-chain/set-cursor-variables

  sound-support:
    plugin: nil
    override-pull: |
      if [ ! -e libcanberra-pulse_*.deb ]; then
        apt-get download libcanberra-pulse:$CRAFT_ARCH_BUILD_FOR;
      fi
    override-build: |
      if [ ! -e $CRAFT_PART_INSTALL/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/libcanberra-*/libcanberra-pulse.so ]; then
        dpkg -x $CRAFT_PART_SRC/libcanberra-pulse_*.deb $CRAFT_PART_INSTALL
      fi

layout:
  /usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/libcanberra-0.30/libcanberra-pulse.so:
    symlink: $SNAP/usr/lib/$CRAFT_ARCH_TRIPLET_BUILD_FOR/libcanberra-0.30/libcanberra-pulse.so
  /usr/share/sounds/Oxygen-Sys-App-Message.ogg:
    symlink: $SNAP/kf5/usr/share/sounds/freedesktop/stereo/dialog-information.oga
```
[/details]

## Publishing the snap

To share your snaps you need to publish them in the Snap Store. First, create an account on [the dashboard](https://dashboard.snapcraft.io/dev/account/). Here you can customise how your snaps are presented, review your uploads and control publishing.

You’ll need to choose a unique "developer namespace" as part of the account creation process. This name will be visible by users and associated with your published snaps.

Make sure the `snapcraft` command is authenticated using the email address attached to your Snap Store account:

```bash
$ snapcraft login
```

### Reserve a name for the snap

You can publish your own version of a snap, provided you do so under a name that's appropriate. For example, if we wanted to publish our KCalc snap, we would need to pick a name that doesn't conflict or cause any confusion with the official [`kcalc` snap published by KDE](https://snapcraft.io/kcalc).

You can register a name on [dashboard.snapcraft.io](https://dashboard.snapcraft.io/register-snap/) or by running the following command:

```bash
$ snapcraft register <snap name>
```

Be sure to update the `name` of your snap (and, if appropriate, the name of your main app) to match the registered name, before running Snapcraft to rebuild the snap.

### Upload the snap

Use Snapcraft to push the snap to the Snap Store. The following command would upload our `kcalc-example` snap to the *edge* release channel:

```bash
$ snapcraft upload --release=edge kcalc-example_*.snap
```

If you’re happy with the result, you should consider committing the *snapcraft.yaml* to a GitHub repository and [turn on automatic builds](https://build.snapcraft.io) so any further commits are built and released to the *edge* release channel automatically. Once you're happy with the quality of your snap, you may want to consider promoting it to a *beta*, *candidate* or *stable* release. You can also use [progressive releases](/t/progressive-releases/20913) to roll out updates to users gradually rather than all at once, helping to limit the number of users who might experience issues if there's an issue with an update.

Congratulations! You've just built and published your first Qt 5 snap. For a more in-depth overview of the snap building process, see [Creating a snap](/t/creating-a-snap/6799).
