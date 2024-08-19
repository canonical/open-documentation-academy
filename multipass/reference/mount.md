<!-- New feedback link at the top of each page!
Please don't copy it blindly, first update the URL passed to the form with the current page URL 
-->

> *Errors or typos? Topics missing? Hard to read? <a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/mount" target="_blank">Let us know</a> or <a href="https://github.com/canonical/multipass/issues/new/choose" target="_blank">open an issue on GitHub</a>.*

> See also: [`mount`](/t/27212), [How to share data with an instance](/t/27189)

<!-- Include link to [ID mapping](/t/45986) once published -->

In Multipass, a **mount** is a directory mapping from the host to an [instance](/t/28469), making its contents, and changes therein, available on both ends. 

In Multipass, there are two types of mounts: classic (default) and native. 

## Classic mounts

Classic mounts use SSHFS (SSH File System) to achieve file/directory sharing. This option is available across all our backends. 

SSHFS is based on SSH, which pays a performance penalty to achieve secure communication.

## Native mounts

Native mounts use driver-dependent technologies to achieve the high performance. They are only available in the following cases:

- On **Hyper-V**, where they are implemented with [SMB/CIFS](https://learn.microsoft.com/en-us/windows/win32/fileio/microsoft-smb-protocol-and-cifs-protocol-overview).
- On **QEMU**, where they are implemented with [9P](https://en.wikipedia.org/wiki/9P_(protocol)).
- On **LXD**, using that backend's own mounts, which also rely on [9P](https://en.wikipedia.org/wiki/9P_(protocol)).

> See also: [Driver (backend) - Feature disparities](/t/28410#feature-disparities)

## Security considerations

[tabs]

[tab version="Linux"]
Because mounts are performed as `root` -- unless installed via snap, see below -- they allow write access to the whole host operating system. But since only privileged users (members of `sudo`, `wheel`, `admin` groups) can use Multipass, this isn't a concern on Linux.

If Multipass is installed via snap package, snap confinement prevents mounts outside of the `/home` directory (and to hidden files/folders in the `/home` directory) and possibly, removable media (depending on connected interfaces). Still, a user (A) with access to Multipass could still access mounts that a different user (B) was able to establish to B's home directory (that is, outside of A's home).
[/tab]

[tab version="macOS"]
Because mounts are performed as `root`, they allow write access to the whole host operating system. But since only privileged users (members of `sudo`, `wheel`, `admin` groups) can use Multipass, this isn't a concern on macOS.
[/tab]

[tab version="Windows"]
Because mounts are performed as privileged users (`SYSTEM` on Windows), they allow write access to the whole host operating system.

For historical reasons, mounts are disabled by default on Windows, even though in the current version of Multipass users need to authenticate with the daemon before it will service their requests. 

<!-- OLD CONTENT 
as anyone with TCP access to `localhost` (`127.0.0.1`) can use Multipass, and by extension, gets access to the entire filesystem. 
-->

[/tab]

[/tabs]
