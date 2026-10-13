An interface enables resources from one snap to be shared with another and with the system. By default, snaps with strict confinement are only able to access a limited set of resources outside the environment they run in. Snaps can only access resources from the system and other snaps via interfaces that describe the resources they provide.

The creator of a snap selects the category of interfaces that a snap requires in order to function correctly. Common categories include [network](/t/the-network-interface/7880), [desktop](/t/the-desktop-interfaces/2042) and the [sound system](/t/the-pulseaudio-interface/7906). <!-- TODO find out how to get the correct URL this link is out of date: should reference https://snapcraft.io/docs/audio-playback-interface>

## Connecting interfaces

A connection between snaps is made when a plug from a snap needing a resource is connected to a slot in another snap providing that resource. An analogy to this is plugging an appliance into an electrical outlet that provides the power it needs.

![A diagram showing two Snaps connected via an interface. The first Snap A is the consumer - shown with a plug. Snap B is the provider - shown with a slot.](https://assets.ubuntu.com/v1/59c290a8-snapd-interfaces.png)

Connections are made either automatically at install time, or manually, depending on their function. The desktop interface is connected automatically, whereas the camera interface isn't. The *Auto-connect* column in the [Supported interfaces](/t/supported-interfaces/7744) table for lists whether an interface automatically connects. See the [Interface Auto-connection mechanism](https://forum.snapcraft.io/t/interface-auto-connection-mechanism/20179) for implementation details.

As with [classic confinement](/t/33649), a snap's publisher can request an *assertion* to automatically connect an otherwise non-auto-connecting interface. For example, the *guvcview* snap [requests](https://forum.snapcraft.io/t/auto-connect-request-for-the-guvcview-brlin-snap/6042) the camera interface automatically-connect when the snap is installed.

* If a snap is upgraded and includes a new assertion, the user will still need to connect the interface manually. Similarly, if a classic snap is upgraded to use strict confinement, its interfaces won't be automatically configured.

* If a snap is installed before an interface is granted auto-connect permission, and the permission is subsequently granted and the snap updated. The installed snap updates, the interface will be auto-connected.
