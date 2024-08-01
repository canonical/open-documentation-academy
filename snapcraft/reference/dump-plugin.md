This plugin dumps the content from a specified source. 

You can specify various details about such a source using [source keywords](/t/snapcraft-parts-metadata/8336#heading--source), such as `source` and `source-type`.

This plugin uses also [common plugin keywords](/t/snapcraft-plugins/8336). 
Such keywords can, for example, be useful when the dumped content needs to be mangled or organised in some way. Using keywords such as `filesets`, `stage`, `snap` and `organize` can be useful in such cases.

For example:

```yaml
parts:
  my-part:
    source: local-source/
    plugin: dump
    organize:
      '*.png' : images/
      launch.wrapper: usr/bin/launcher
    prime:
      - -README*
  remote-part:
    plugin: dump
    source: https://remote-resource.org/cool-package.deb
    source-type: deb
```

See [source keywords](/t/snapcraft-parts-metadata/8336#heading--source) and [common plugin keywords](/t/snapcraft-plugins/4284) for more information.

For more examples, click here to find [GitHub](https://github.com/search?o=desc&q=path%3Asnapcraft.yaml+%22plugin%3A+dump%22+&s=indexed&type=Code&utf8=%E2%9C%93) projects already using this plugin.

> ⓘ  This is a *snapcraft* plugin. See [Snapcraft plugins](/t/snapcraft-plugins/4284) and [Supported plugins](/t/supported-plugins/8080) for further details on how plugins are used.
