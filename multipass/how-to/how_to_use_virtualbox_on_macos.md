tmihoc | 2024-06-09 08:11:00 UTC | #1

<!-- New feedback link at the top of each page!
Please don't copy it blindly, first update the URL passed to the form with the current page URL 
-->

> *Errors or typos? Topics missing? Hard to read? <a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/use_virtualbox_on_mac.os" target="_blank">Let us know</a> or <a href="https://github.com/canonical/multipass/issues/new/choose" target="_blank">open an issue on GitHub</a>.*

This document demonstrates how to "Use VirtualBox to set up port forwarding for a Multipass instance.
Multipass runs as the `root` user, so to see the instances in  VirtualBox, or through the `VBoxManage` command, you have to run those as `root`, too. To see the instances in VirtualBox, execute:

```plain
$ sudo VirtualBox
```

![Multipass instances in VirtualBox](upload://mld2SIalX93tc3StGHaqb6OTyDO.png) 

And, to list the instances on the command line, execute:

```plain
$ sudo VBoxManage list vms
"primary" {395d5300-557d-4640-a43a-48100b10e098}
```

[note type="information"]
You can still use the `multipass` client and the system menu icon, and any changes you make to the configuration of the instances in VirtualBox will be persistent. They may not be represented in Multipass commands such as `multipass info` , though.
[/note]

---
*<a href="https://docs.google.com/forms/d/e/1FAIpQLSd0XZDU9sbOCiljceh3rO_rkp6vazy2ZsIWgx4gsvl_Sec4Ig/viewform?usp=pp_url&entry.317501128=https://multipass.run/docs/use_virtualbox_on_macos" target="_blank">Let us know</a> how this worked for you and what you’d like to see next!*

-------------------------