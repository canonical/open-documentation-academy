[note]

Snapcraft extensions simplify and streamline the process of adding commonly used elements to a snap, such as libraries, themes and environment variables. See [Snapcraft extensions](/t/snapcraft-extensions/13486) for further details.
[/note]

The `kde-neon` extension helps developers to create snaps that use [Qt 5](https://doc.qt.io/qt-5/), with or without the add-on [KDE Frameworks 5](https://kde.org/products/frameworks/) libraries.

The extension is maintained and supported for use with the `core22` base snap. Whilst older versions of the extension are still available for `core20` and `core18`, these bases are no longer actively supported.

The rest of this document focuses on the current version of the `core22` extension:

| Base | Versions | Platform snap | Build snap |
|-|----|----|----|
| core22 | Qt 5.15.11 and KDE Frameworks 5.113 | [kf5-5-113-qt-5-15-11-core22](https://snapcraft.io/kf5-5-113-qt-5-15-11-core22) | [kf5-5-113-qt-5-15-11-core22-sdk](https://snapcraft.io/kf5-5-113-qt-5-15-11-core22-sdk) |

[note type="caution"]

The extension is designed for C++ based Qt/KDE Frameworks applications. It does not provide the bindings needed for *Qt for Python* (*PySide2*) or *PyQt* applications. In addition, the extension does not provide all of the optional Qt 5 libraries; for example, it does not include Qt3D, QtCharts, QtDataVisualization or QtGamepad.
[/note]

## How to use it

To use the extension, add the `extensions` keyword with the value `kde-neon` to each app defined in your `snapcraft.yaml` file. For example:

```yaml
apps:
  kcalc-example:
    command: usr/bin/kcalc
    extensions:
      - kde-neon
    ...
```

See [Qt 5 and KDE Frameworks applications](/t/qt5-and-kde-frameworks-applications/13753) for an example of how to use this extension to build a Qt 5 and KDE Frameworks 5 based application.

## Interface connections

The extension connects your snap to the following run-time content snaps:

- [`kf5-5-113-qt-5-15-11-core22`](https://snapcraft.io/kf5-5-113-qt-5-15-11-core22) for the Qt 5 and KDE Frameworks run-time libraries
- [`gtk-common-themes`](https://snapcraft.io/gtk-common-themes) for common icon, cursor and sound themes

The extension achieves this by adding the following plugs to your *snapcraft.yaml* at build time:

```yaml
plugs:
    desktop:
        mount-host-font-cache: false
    icon-themes:
        interface: content
        target: $SNAP/data-dir/icons
        default-provider: gtk-common-themes
    sound-themes:
        interface: content
        target: $SNAP/data-dir/sounds
        default-provider: gtk-common-themes
    kf5-5-113-qt-5-15-11-core22:
        content: kf5-5-113-qt-5-15-11-core22-all
        interface: content
        default-provider: kf5-5-113-qt-5-15-11-core22
        target: $SNAP/kf5
```

In addition, the following plugs are added to each app that includes the `kde-neon` extension entry:

```yaml
apps:
  <app name>:
    ...
    plugs:
      - desktop
      - desktop-legacy
      - opengl
      - wayland
      - x11
    ...
```

See [Adding interfaces](/t/adding-interfaces/13123) for more details.

## Included packages

The `kde-neon` extension depends on two separate snaps: a build snap and a platform/content snap.

The _build snap_ ensures that the relevant Qt 5 and KDE Frameworks development libraries and supporting files are available during the build process. These libraries are sourced from the Ubuntu-based [*KDE neon* Linux distribution](https://neon.kde.org/), which provides more recent versions of Qt 5 and the KDE Frameworks than are available in the `core22` software archive.

The _platform snap_ makes the corresponding run-time libraries available to your snap when it is launched by your users. If the _platform snap_ isn't already present on a user's machine, then it will be installed automatically and simultaneously with the `kde-neon` extension based snap.

By relying on a standalone _platform snap_, developers can avoid bundling the Qt/KDE Frameworks libraries in their snap, keeping the file size of the snap to a minimum. The same installation of the _platform snap_ can be used by multiple snaps that rely on the `kde-neon` extension.

## The build environment

The `kde-neon` extension defines the `PATH`, `XDG_DATA_DIRS` and `SNAPCRAFT_CMAKE_ARGS` build-time environment variables by adding a `build-environment` section to each of your snap's build parts:

```yaml
build-environment:
-   PATH: /snap/kf5-5-113-qt-5-15-11-core22-sdk/current/usr/bin${PATH:+:$PATH}
-   XDG_DATA_DIRS: $CRAFT_STAGE/usr/share:/snap/kf5-5-113-qt-5-15-11-core22-sdk/current/usr/share:/usr/share${XDG_DATA_DIRS:+:$XDG_DATA_DIRS}
-   SNAPCRAFT_CMAKE_ARGS: -DCMAKE_FIND_ROOT_PATH=/snap/kf5-5-113-qt-5-15-11-core22-sdk/current${SNAPCRAFT_CMAKE_ARGS:+:$SNAPCRAFT_CMAKE_ARGS}
```

You can override these defaults and/or define other build-time environment variables on a part-by-part basis by adding your own `build-environment` sections to *snapcraft.yaml*.

## The run time environment

The extension also sets various run-time environment variables.

Most of these variables are set at launch by a `command-chain` shell script named `desktop-launch`:

```yaml
apps:
  <app name>:
    ...
    command-chain:
    - snap/command-chain/desktop-launch
    ...
```

This shell script is added to the snap by a build part named `kde-neon/sdk`:

```yaml
parts:
  kde-neon/sdk:
      source: /snap/snapcraft/current/share/snapcraft/extensions/desktop/kde-neon
      plugin: make
      make-parameters:
      - PLATFORM_PLUG=kf5-5-113-qt-5-15-11-core22
      build-snaps:
      - kf5-5-113-qt-5-15-11-core22-sdk
```

This part assembles the `desktop-launch` script from the following component scripts:
- [init](https://github.com/canonical/snapcraft/blob/main/extensions/desktop/common/init)
- [desktop-exports](https://github.com/canonical/snapcraft/blob/main/extensions/desktop/common/desktop-exports)
- [launcher-specific](https://github.com/canonical/snapcraft/blob/main/extensions/desktop/kde-neon/launcher-exec)
- [mark-and-exec](https://github.com/canonical/snapcraft/blob/main/extensions/desktop/common/mark-and-exec)

In addition to running the `desktop-launch` script at run time, the extension sets `SNAP_DESKTOP_RUNTIME` using an `environment` parameter:
```yaml
environment:
  SNAP_DESKTOP_RUNTIME: $SNAP/kf5
```

### Layout

The extension sets the following `layout`:

```yaml
layout:
  /usr/share/X11:
    symlink: $SNAP/kf5/usr/share/X11
```

### Hooks

The extension defines a [configure hook](/t/supported-snap-hooks/3795) that runs a script named `hooks-configure-desktop` upon installation of the snap, every time the snap is refreshed, and whenever the user changes a configuration option using `snap set` or `snap unset`:

```yaml
hooks:
  configure:
    plugs:
    - desktop
    command-chain:
    - snap/command-chain/hooks-configure-desktop
```

The `hooks-configure-desktop` script is a copy of [this `fonts` script](https://github.com/canonical/snapcraft/blob/main/extensions/desktop/common/fonts).
