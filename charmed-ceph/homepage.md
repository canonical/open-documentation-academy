---

Charmed Ceph is a software-defined storage solution that provides object, block and file storage on commodity hardware.

Charmed Ceph makes it easy to use and deploy [Ceph][upstream-ceph]. It provides a simpler way to deliver Ceph to users using a set of scripts called charms that are deployed with [Juju](https://juju.is/).

Corporate users can benefit from the robust and manageable full-suite storage solution, and the ease with which their Ceph clusters can be deployed with Charmed Ceph.

An individual or corporate user with a large and growing volume of valuable data to manage, and who appreciates the ease of integration with other cloud services, would greatly benefit from Charmed Ceph.

---

## In this documentation

|  |  |
|--|--|
| **Tutorial** </br> [Get Started][gs]: a hands-on introduction to Ceph clusters for new users | **How-to guides** </br> [How-to guides][ht] for installation, integration and other key operations |
| **Explanation** </br> Technical background, discussions and clarification of key topics, including [pool types][pt] and [storage types](https://ubuntu.com/ceph/docs/explanation#storage-types)| **Reference** </br> Technical information: [supported versions](/t/supported-ceph-versions/18799), [release cycle](/t/release-cycle/18800), [release notes](/t/ceph-release-notes/18764) and [appendices](/t/release-cycle/18800)|

---

## Project and community

Charmed Ceph is an Open Source project that welcomes usage discussion, project feedback, and especially contributions!

* Join our [user forum][juju-discourse-openstack]
* Chat to us on [Matrix](https://app.element.io/#/room/#ceph-general:ubuntu.com)
* We abide by the [Ubuntu Code of Conduct][ubuntu-coc]
* Get involved in improving our [software][ceph-charm-repos] or [documentation](/t/39095)

Don't hesitate to [contact us][contact-ceph] if you have questions about integrating Charmed Ceph into your project.

<!-- LINKS -->

[upstream-ceph]: https://ceph.com/en/

[gs]: /t/getting-started/18801
[ht]: /t/31143
[pt]: /t/pool-types/30609
[rns]: /t/ceph-release-notes/18764
[apps]: /t/appendices/30626
[ubuntu-coc]: https://ubuntu.com/community/ethos/code-of-conduct
[juju-discourse-openstack]: https://discourse.charmhub.io/tag/openstack
[contact-ceph]: https://ubuntu.com/ceph/contact-us
[ceph-charm-repos]: https://opendev.org/explore/repos?sort=recentupdate&language=&q=charm-ceph&only_show_relevant=false

<!-- MENU -->

## Navigation

[details=Navigation]

|Level | Path | Navlink|
|--- | --- | ---|
|0 | home | [Home](/t/17250)|
|0 | tutorial | [Tutorial](/t/56554)|
|0 | howtos | [How-to guides](/t/31143)|
|1 |  | Installing Ceph|
|2 | install-ceph | [Charmed Ceph](/t/18803)|
|2 | install-dashboard | [Ceph dashboard](/t/24829)|
|1 |  | Managing Ceph cluster|
|2 | adding-osds | [Adding OSDs](/t/18783)|
|2 | adding-mons | [Adding MONs](/t/18784)|
|2 | replacing-osd-disks | [Replacing OSD disks](/t/18788)|
|2 | removing-osds | [Removing OSDs](/t/28296)|
|2 | removing-mons | [Removing MONs](/t/27595)|
|2 | enabling-fde | [Enabling full disk encryption](/t/68577)|
|1 |  | Multi-site operations|
|2 | setting-up-multi-site-replication | [Setting up multi-site replication](/t/36025)|
|2 | recovering-from-an-outage-with-multi-site-replication | [Recovering from an outage with multi-site replication](/t/36026)|
|2 | scaling-down-multi-site-to-single-site | [Scaling down multi-site to single-site](/t/36027)|
|1 | node-startup-and-shutdown | [Node startup and shutdown](/t/18780)|
|1 | connecting-to-the-observability-stack | [Connecting to the observability stack](/t/45534)|
|1 | integrating-ceph-with-other-tools | Integrating Ceph with other tools|
|2 | kubernetes | [Kubernetes](/t/18794)|
|2 | lxd | [LXD](/t/18793)|
|2 | openstack | [OpenStack](/t/18792)|
|2 | vmware | [VMware](/t/18791)|
|1 | upgrading-deployment-software | [Upgrading deployment software](/t/18769)|
|2 | upgrading-charms | [Upgrading charms](/t/18776)|
|2 | upgrading-nodes | [Upgrading node operating system](/t/18777)|
|2 | upgrading-ceph | [Upgrading Ceph](/t/18778)|
|1 | contact-us | Contact us|
|2 | report-security-vulnerabilities | [Report security vulnerabilities](/t/68578)|
|0 | explanation | [Explanation](/t/47709)|
|1 | security | [Security in Charmed Ceph](/t/60203)|
|2 | attack-surface | [Attack surface](/t/69018)|
|2 | access-controls | [Access controls](/t/69019)|
|2 | secrets | [Secrets](/t/69020)|
|2 | encryption | [Encryption](/t/69021)|
|2 | secure-deployment | [Best practices for secure deployment](/t/69022)|
|2 | secure-operation | [Best practices for secure operation](/t/69023)|
|2 | cryptographic-technologies-in-charmed-ceph | [Cryptography in Charmed Ceph](/t/59119)|
|2 | enabling-full-disk-encryption | [Enabling full disk encryption](/t/18779)|
|1 | ceph-architecture | [Ceph architecture](/t/69017)|
|1 | object-storage | [Object storage](/t/18819)|
|1 | block-storage | [Block storage](/t/18821)|
|1 | file-storage | [File storage](/t/18818)|
|1 | replicated-pools | [Replicated pools](/t/18805)|
|1 | erasure-coded-pools | [Erasure coded pools](/t/18807)|
|1 | multi-site-replication | [RADOS Gateway multi-site replication](/t/35992)|
|0 | reference | [Reference](/t/47708)|
|1 | supported-ceph-versions | [Supported Ceph versions](/t/18799)|
|1 | release-notes | [Release notes](/t/18764)|
|1 | appendices | [Appendices](/t/30626)|
|2 | client-setup | [Client setup](/t/18765)|
|2 | charm-list | [Charm list](/t/18766)|
|2 | upgrade-notes | [Upgrade notes](/t/18767)|
|0 | contribute-to-our-docs | [Contribute to our docs](/t/58465)|
| | doc-contrib | [Documentation contributions](/t/39095)|

[/details]
