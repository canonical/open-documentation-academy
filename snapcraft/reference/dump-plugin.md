The `dump` plugin copies the contents of a *source* into the root directory of the snap, without running any language-specific packaging logic. It provides an easy way to add pre-built binaries, scripts and other assets to a snap.

The *source* is defined using the [common keywords for sources](/t/snapcraft-parts-metadata/8336#heading--source).

The following example shows how the `dump` plugin can be used with a remote compressed tarball:

```yaml
parts:
  my-remote-part:
    plugin: dump
    source: https://www.example.com/builds/latest-build.tar.gz
```

The behaviour of the `dump` plugin can be modified using optional [common plugin keywords](/t/snapcraft-parts-metadata/8336). For example:
* the `organize` keyword can be used to move and rename files and directories contained in the source;
* the `stage` and `prime` keywords can be used to specify which certain files and directories should (and should not) be copied into the snap; and
* the `override-build`, `override-stage` and `override-prime` keywords provide a way of running [custom shell scripts](/t/overrides/4892) during the build process

These keywords are particularly useful when building a snap from multiple component parts, where the main application expects to find supporting binaries, libraries and assets (such as themes or plugins) in specific locations. They are also useful when the source contains components for multiple [architectures](/t/architectures/4972), where it is only necessary to copy files for one architecture into the snap.

Note that the `organize` keyword is processed *before* `stage` and `prime`. This means that any `stage` and `prime` entries should refer to the files as renamed and/or moved, and not to the original names and locations.

The following example is based on a source tarball that contains directories named `binaries`, `docs` and `etc`, as well as files named `COPYRIGHT` and `README`. The `organize` keyword moves the `binaries` directory and `COPYRIGHT` file to `usr/bin` and `usr/share/doc/my-app/COPYRIGHT` respectively. The `stage` keyword copies everything other  than the `docs` directory and `README` file into the snap.

```yaml
parts:
  my-app-part:
    plugin: dump
    source: https://www.example.com/builds/latest-build.tar.gz
    organize:
      binaries: usr/bin
      COPYRIGHT: usr/share/doc/my-app/COPYRIGHT
    stage:
      - -README
      - -docs
```

For more examples, see [Pre-built apps](/t/pre-built-apps/6739) or [search for GitHub projects](https://github.com/search?o=desc&q=path%3Asnapcraft.yaml+%22plugin%3A+dump%22+&s=indexed&type=Code&utf8=%E2%9C%93) already using this plugin.

> ⓘ  This is a *snapcraft* plugin. See [Snapcraft plugins](/t/snapcraft-plugins/4284) and [Supported plugins](/t/supported-plugins/8080) for further details on how plugins are used.
> 
