This section contains guides to help you manage you have your already up and running Ceph cluster, taking you through the complete node lifecycle.

## Installing and initialising Ceph

Perform a general install of Charmed Ceph access the web UI.

* [Installing Charmed Ceph](/t/charmed-ceph-manual-install/18803)
* [Installing Ceph dashboard](/t/ceph-dashboard-install/24829)

<!-- ## Node lifecycle-->
## Managing your cluster
To ensure the health of your cluster, you need to scale your OSDs and MONs up or down, or replace OSD disks by recreating a Ceph OSD disk within a Charmed Ceph deployment. Depending on your MAAS nodes, you may need to change your change block devices.

  - [Adding OSDs](/t/18783)
  - [Adding MONs](/t/18784)
  - [Replacing OSD disks](/t/18788)
  - [Removing OSDs](/t/28296)
  - [Removing MONs](/t/27595)
  - [Enabling full disk encryption](/t/68577)

## Multi-site operations

Here we discuss setting up [RADOS Gateway multi-site replication](https://ubuntu.com/ceph/docs/multi-site-replication) in a charmed Ceph deployment. Native replication between ceph-radosgw applications is supported via `juju relations`. By default, both primary and secondary RGW applications accept write operations (i.e. active-active replication is configured).

* [Setting up multi-site replication](https://ubuntu.com/ceph/docs/setting-up-multi-site-replication)
* [Recovering from an outage with multi-site replication](https://ubuntu.com/ceph/docs/recovering-from-an-outage-with-multi-site-replication)
* [Scaling down multi-site to single-site](https://ubuntu.com/ceph/docs/scaling-down-multi-site-to-single-site)

## Upgrading deployment software

There are three pieces of software in a Charmed Ceph deployment that can be upgraded:

1. [charms](/t/18776)
2. [cluster node operating system](/t/18777)
3. [Ceph itself](/t/18778)

## Contact us

  - [Report security vulnerabilities](/t/68578)
  
