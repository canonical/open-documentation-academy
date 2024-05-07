By default, snaps with strict confinement are only able to access a limited set of resources outside the environment they run in. Snaps can only access resources from the system and other snaps via interfaces that describe the resources they provide.

The creator of a snap selects the interfaces that a snap requires in order to function correctly. Common interfaces include those that provide access to the [network](/t/the-network-interface/7880), [desktop features](/t/the-desktop-interfaces/2042) and the [sound system](/t/the-pulseaudio-interface/7906).

## Connecting interfaces

A connection between snaps is made when a plug from a snap needing a resource
is connected to a slot in another snap providing that resource.
A analogy to this is plugging an appliance into an electrical outlet that
provides the power it needs.

![How an interfaces uses a plug and a slot](https://assets.ubuntu.com/v1/59c290a8-snapd-interfaces.png) 

Connections are made either automatically at install time or manually, depending on their function. The desktop interface is connected automatically, for instance, whereas the camera interface is not. The *Auto-connect* column in the [Supported interfaces](/t/supported-interfaces/7744) table for lists  whether an interface automatically connects or not. See the [Interface Auto-connection mechanism](https://forum.snapcraft.io/t/interface-auto-connection-mechanism/20179) for implementation details.

As with [classic confinement](/t/33649), a snap's publisher can request an *assertion* to automatically connect an otherwise non-auto-connecting interface. For example, the *guvcview* snap [requested](https://forum.snapcraft.io/t/auto-connect-request-for-the-guvcview-brlin-snap/6042) the camera interface be automatically-connected when the snap is installed.

* If a snap is upgraded and includes a new assertion, the user will still need to connect the interface manually. Similarly, if an installed classic snap is upgraded to use strict confinement, its interfaces won't be automatically configured.

* If a snap is installed prior to an interface being granted auto-connect permission, and permission is subsequently granted and the snap updated, when the installed snap updates, the interface will be auto-connected.

## Getting the interfaces for a snap

Use the `snap connections` command to see which interfaces a snap needs, and which are currently connected:

```bash
$ snap connections vlc
Interface         Plug                  Slot               Notes
camera            vlc:camera            -                  -
desktop           vlc:desktop           :desktop           -
desktop-legacy    vlc:desktop-legacy    :desktop-legacy    -
home              vlc:home              :home              -
mount-observe     vlc:mount-observe     -                  -
[...]
```

In the above example,  we can see that the `vlc:camera` interface is disconnected because it has an empty *Slot* entry.

See [Interface management](/t/interface-management/6154) for further interface details, including how to disconnect interfaces and make manual connections, and [Security policy and sandboxing](https://forum.snapcraft.io/t/security-policy-and-sandboxing/554) for more information on how confinement is implemented.