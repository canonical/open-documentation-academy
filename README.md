# Open Documentation Academy

*Discover open source through documentation*

The Open Documentation Academy combines Canonical’s documentation team with documentation newcomers, experts, and those in-between, to help us all improve documentation practice and become better writers. Fill blanks in your resume and paint your GitHub activity tracker golden.

If you're a newcomer, we can provide help, advice, mentorship, and a hundred different tasks to get started on. If you're an expert, we want to create a place to share knowledge, a place to get involved with new developments, and somewhere you can ask for help on your own projects.

A key aim of this initiative is to help lower the barrier into successful open-source software contribution, by making documentation into the gateway.

[Join the academy HERE](https://discourse.ubuntu.com/c/open-documentation-academy/166)

## This repository

The purpose of this repository is to list and track global documentation tasks. These are filed as _issues_ in this repository. Tasks vary from broken formatting and missing documentation, to updates, re-structuring, and rewriting.

Issues are identified and shared by participating projects at Canonical who control whether an issue is merged into their documentation. An academy participant and a mentor work together to guide a contribution through to completion.

### Participating projects

The first words of an issue's title will typically indicate the project it involved. These include the following:

- [ADSys](https://documentation.ubuntu.com/adsys/en/stable/): Active Directory Group Policy client for Ubuntu
- [Anbox Cloud](https://anbox-cloud.io/docs): Solution offering scalable Android in the cloud
- [Canonical Kubernetes](https://ubuntu.com/kubernetes/docs): the reference platform for Kubernetes on all major public clouds
- [Charmed OpenStack](https://ubuntu.com/openstack/docs): our traditional enterprise cloud solution
- [Juju](https://juju.is/docs):  open source orchestration engine
- [LXD](https://documentation.ubuntu.com/lxd/en/latest/): open source container and VM management at any scale
- [Landscape](https://ubuntu.com/landscape/docs): Ubuntu systems management, monitoring and administration platform
- [Launchpad](https://documentation.ubuntu.com/launchpad/en/latest/): software development lifecycle and collaboration platform
- [MAAS](https://maas.io/docs): bare metal cloud with on-demand servers
- [MicroCeph](https://canonical-microceph.readthedocs-hosted.com/en/squid-stable/): the easiest way to get up and running with Ceph
- [MicroStack](https://microstack.run/docs): our next generation enterprise cloud solution
- [Multipass](https://discourse.ubuntu.com/t/multipass-documentation/8294): tool to generate cloud-style Ubuntu virtual machines
- [Netplan](https://github.com/canonical/netplan): network configuration for various backends
- [Our Sphinx and RST starter pack](https://github.com/canonical/sphinx-docs-starter-pack): our open source template for building modern documentation
- [Snap and Snapcraft](https://snapcraft.io/docs): Linux app packages and the build tools for desktop, cloud and IoT
- [Ubuntu Developer Guide](https://github.com/canonical/ubuntu-for-developers-docs): guide for developers using Ubuntu Desktop as a development platform
- [ubuntu-image](https://github.com/canonical/ubuntu-image): Tool for generating bootable Ubuntu images
- [Ubuntu on public cloud](https://documentation.ubuntu.com/public-cloud/en/latest/): Optimised Ubuntu images for partner clouds
- [Ubuntu Packaging Guide](https://github.com/canonical/ubuntu-packaging-guide): manual for Ubuntu package maintainers
- [Ubuntu Server documentation](https://github.com/canonical/ubuntu-server-documentation): Official documentation for the Ubuntu Server distribution
- [Ubuntu WSL](https://canonical-ubuntu-wsl.readthedocs-hosted.com/en/latest/): Ubuntu terminal environment on Windows with the Windows Subsystem for Linux (WSL)

This list will expand as more projects get involved. We're also happy to include projects outside of Canonical.

### Contributor licence agreement

Many of the projects that participate in the Open Documentation Academy require that a contributor has signed a _Contributor licence agreement_, or CLA. Such an agreement will typically grant permission for the project to use a contribution while the contributor retains the copyright and the rights to modify their own work, or use it in other projects.

The [Canonical contributor licence agreement](https://ubuntu.com/legal/contributors) is one such CLA. This [needs to be signed](https://ubuntu.com/legal/contributors/agreement) before a contribution can be considered for inclusion within one of Canonical's projects. Many GitHub repositories for Canonical projects will  automatically check whether a contributor has signed the CLA when a contribution is made.

The `cla` issue label is used to help identify which tasks require a contributor to have signed a CLA.

### Time considerations

We’re completely flexible when it comes to how much time a task may take a contributor. Take as little or as much time as you need.

However, we do ask that potential contributors indicate an estimated target date. This helps us to better manage the task list and to ensure tasks are being actively worked on. If you need to change your estimate, please let us know because it won’t be a problem. Similarly, let us know if you are unable to work on a task for a period of time. A comment attached to the task is enough.

If there has been no activity on a task for several weeks, we'll initially reach out to the assignee before releasing the task back into the pool of unassigned tasks.

### Issue labels

We use one or more of the following issue labels both for consistency and to indicate what might be expected from a task.

#### https://github.com/canonical/open-documentation-academy/labels/cla

Identifies tasks that require a contributor to have signed a [CLA](#contributor-licence-agreement).

#### https://github.com/canonical/open-documentation-academy/labels/code

Used for tasks that may require some programming knowledge, or a programmatic solution.

#### https://github.com/canonical/open-documentation-academy/labels/di%C3%A1taxis

Revise a document to better conform to a [Diátaxis](https://diataxis.fr/) type:

- Tutorial
- How-to
- Reference
- Explanation

This may require a document to be split, edited, or sometimes re-written.

#### https://github.com/canonical/open-documentation-academy/labels/edit

Edit pre-existing documentation for consistency, accuracy, style and application.

#### https://github.com/canonical/open-documentation-academy/labels/explanation

Create or revise a document to better reflect an understanding-oriented [explanation](https://diataxis.fr/explanation/).

#### https://github.com/canonical/open-documentation-academy/labels/good%20first%20issue

An ideal task to start with. Marking issues with this label is a widely adopted [GitHub convention](https://github.com/topics/good-first-issue).

#### https://github.com/canonical/open-documentation-academy/labels/help%20wanted

Another [GitHub convention](https://github.com/topics/help-wanted) to indicate that a project welcomes community help with an issue. 

#### https://github.com/canonical/open-documentation-academy/labels/how-to

Create or revise a document to better reflect a [how-to guide](https://diataxis.fr/how-to-guides/) to achieve a specific goal.

#### https://github.com/canonical/open-documentation-academy/labels/new

Adding new or missing documentation for a specific tool, feature, or function.

#### https://github.com/canonical/open-documentation-academy/labels/oda-admin

Tasks relating to the admin of the Open Documentation Academy (ODA) project.

#### https://github.com/canonical/open-documentation-academy/labels/reference

Create or revise a document to better reflect a technical description to use as [reference](https://diataxis.fr/reference/) material.

#### https://github.com/canonical/open-documentation-academy/labels/review

Review pre-existing documentation for quality, accuracy and consistency. This work may require small updates to the original documentation and/or the creation of sub-tasks to address any detected and substantial shortcomings.

#### https://github.com/canonical/open-documentation-academy/labels/size%201 https://github.com/canonical/open-documentation-academy/labels/size%202 https://github.com/canonical/open-documentation-academy/labels/size%203 https://github.com/canonical/open-documentation-academy/labels/size%205 https://github.com/canonical/open-documentation-academy/labels/size%208

This is our estimation of effort and complexity. Size values range from 1 to 8, representing _least effort_ to _most effort_ respectively. These numbers follow the [Fibonacci ### sequence](https://en.wikipedia.org/wiki/Fibonacci_sequence) sequence of 1, 2, 3, 5, 8, with size 8 likely to be a significant undertaking.

#### https://github.com/canonical/open-documentation-academy/labels/ta%20wanted

The technical author (TA) team at Canonical wants to help projects without access to documentation experts. This label is used for such projects to mark tasks any technical author can help with.

#### https://github.com/canonical/open-documentation-academy/labels/tutorial

Develop, write, edit or update a [tutorial](https://diataxis.fr/tutorials/). Tutorials are often the hardest kinds of documentation to write or update because they primarily require good teaching skills and perception, before you even start writing.

#### https://github.com/canonical/open-documentation-academy/labels/update

Update potentially outdated instructions, commands, or version numbers. These tasks might include release notes, version numbers, new command line arguments and features, and even complete overhauls when a major release occurs.

## Further resources

If you're new to GitHub and working on the command line, you may want to start off with our [getting started guide](getting-started/get_started.md). Even if you are running a Windows machine, you can start contributing using this guide.

### Community forum

Our community forum is the hub for all things Open Documentation Academy. It includes our _Getting started_ guide and links to our weekly _Documentation office hours_, alongside meeting notes, updates, external links and discussions.

https://discourse.ubuntu.com/c/community/open-documentation-academy/166

### Synchronous chat

For more interactive chat, the documentation team can be found on [Matrix](https://matrix.org/).

<https://matrix.to/#/#documentation:ubuntu.com>

### Social media

You can find us on [Fosstodon](https://fosstodon.org/explore), where we post frequent updates related to the _Academy_ and our other documentation initiatives.

<https://fosstodon.org/@CanonicalDocumentation>

### Calendar

Subscribe to our [Documentation event calendar](https://calendar.google.com/calendar/u/0?cid=Y19mYTY4YzE5YWEwY2Y4YWE1ZWNkNzMyNjZmNmM0ZDllOTRhNTIwNTNjODc1ZjM2ZmQ3Y2MwNTQ0MzliOTIzZjMzQGdyb3VwLmNhbGVuZGFyLmdvb2dsZS5jb20). Not only does this include our _Documentation office hours_, it will also include any other discussion or training events we organise.
