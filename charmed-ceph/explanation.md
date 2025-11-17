This section provides explanatory and conceptual guides as well as a technical background on topics. It is meant to help you expand your knowledge of the Ceph ecosystem.

## Storage types

Ceph provides object, block, and file based storage on commodity hardware.

* [Object storage](/t/18819)
* [Block storage](/t/18821)
* [File storage](/t/18818)

## Pool types

Pools constitute the basic building blocks of a Ceph cluster. They allow for high level organisation of data into logical storage areas, where each is defined by a number of properties (e.g. quantity of Placement Groups, CRUSH rules, resiliency factor). A pool can be one of two types:

* [Replicated pools](/t/18805)
* [Erasure-coded pools](/t/18807)

## RADOS Gateway multi-site replication

Charmed Ceph supports multi-site replication with the Ceph RADOS Gateway. This provides even more resilience to your object storage by replicating it to geographically separated Ceph clusters. This is of particular importance for active backup and disaster recovery scenarios where a site is compromised and access to data would be otherwise lost.

 * [RADOS Gateway multi-site replication](/t/35992)

## Security

Learn about security topics applicable to Charmed Ceph.

* [Full disk encryption](/t/68576)
