<!-- New feedback link at the top of each page!
Please don't copy it blindly, first update the URL passed to the form with the current page URL 
-->

> *Errors or typos? Topics missing? Hard to read? <a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/mount-command" target="_blank">Let us know</a> or <a href="https://github.com/canonical/multipass/issues/new/choose" target="_blank">open an issue on GitHub</a>.*

> See also: [Mount](/t/28470), [How to share data with an instance](/t/27189), [`umount`](/t/27214), [ID mapping](/t/45986).

The `multipass mount` command maps a local directory from the host to an instance, with the possibility to specify the [mount](/t/28470) type (classic or native) and define group or user [ID mappings](/t/45986). Here's the syntax for mapping a local directory to your virtual machine using the classic mount:

```shell
multipass mount --type=classic /host/path instance:/instance/path
```
The `classic` mounts use technology built into multipass and allows for higher compatibility. However, it sacrifices performance. The `native` mounts on the other hand use hypervisor and/or platform specific mounts to offer better performance but has [limited compatibility](https://multipass.run/docs/mount#native-mounts).

Use the `multipass umount` command to undo the mapping.

---

The full `multipass help mount` output explains the available options:

```plain
$ multipass help mount
Usage: multipass mount [options] <source> <target> [<target> ...]
Mount a local directory inside the instance. If the instance is
not currently running, the directory will be mounted
automatically on next boot.

Options:
  -h, --help                       Displays help on commandline options
  -v, --verbose                    Increase logging verbosity. Repeat the 'v'
                                   in the short option for more detail. Maximum
                                   verbosity is obtained with 4 (or more) v's,
                                   i.e. -vvvv.
  -g, --gid-map <host>:<instance>  A mapping of group IDs for use in the mount.
                                   File and folder ownership will be mapped from
                                   <host> to <instance> inside the instance. Can
                                   be used multiple times. Mappings can only be
                                   specified as a one-to-one relationship.
  -u, --uid-map <host>:<instance>  A mapping of user IDs for use in the mount.
                                   File and folder ownership will be mapped from
                                   <host> to <instance> inside the instance. Can
                                   be used multiple times. Mappings can only be
                                   specified as a one-to-one relationship.
  -t, --type <type>                Specify the type of mount to use.
                                   Classic mounts use technology built into
                                   Multipass.
                                   Native mounts use hypervisor and/or platform
                                   specific mounts.
                                   Valid types are: 'classic' (default) and
                                   'native'

Arguments:
  source                           Path of the local directory to mount
  target                           Target mount points, in <name>[:<path>]
                                   format, where <name> is an instance name, and
                                   optional <path> is the mount point. If
                                   omitted, the mount point will be the same as
                                   the source's absolute path
```
