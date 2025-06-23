# What is Ubuntu Desktop made of?

Ubuntu Desktop has millions of users today. As a new user, you might be curious to know what Ubuntu Desktop is made of.
Not necessarily a deep technical explanation, but a great overview of what it is. This is what this guide addresses.
It explains what a desktop environment is, what applications are, how package managers work, and more.

## Desktop environment

The Ubuntu Desktop environment provides a Graphical User Interface (GUI) for interacting with Ubuntu.
Think of it like a theme but with more in-depth customization. Several desktop environments exist, and some are memory-intensive and others aren't.
Ubuntu Desktop ships with the GNOME desktop environment by default. Other alternative environments are:

- KDE Plasma
- XFCE
- LXQt
- MATE
- Budgie, etc.

Interestingly, Ubuntu Desktop comes in [different flavors](https://ubuntu.com/desktop/flavours), each with its desktop environment.
For example, the Kubuntu flavor supports the KDE Plasma desktop environment.

A desktop environment consists of different independent components that are customizable.
Sometimes, a Linux user may choose to run only one component rather than the entire desktop environment to save memory. Anyway, some of these components are:

- **Windows manager**: This manages a window that pops up when you open a terminal or any application at all.
- **User app**: These are default applications in the desktop environment.
- **File manager**: This is a UI environment for interacting with files.

## Applications

This comprises of system and user applications.

- **System application**: The system applications interact with the operating system. Examples are App Center, Terminal, etc.
- **User applications**: They perform day-to-day activities and have permissions to the operating system restricted. One example is Firefox.

## Package manager

A package manager is used to manage your application lifecycle; install, upgrade, remove, etc. You can do this using a GUI or command line.

- **The GUI**: Ubuntu Desktop comes pre-installed with an application store called App Center or Snap Store. For example, you open it, search for an application, and install it.
- **Command line**: This is the backend version of the GUI application store. It uses `apt` or `snap`, and the applications are downloaded from a repository; this may be one maintained by Canonical or a third party.
  - **apt (Advanced Package Tool)**: This isn't Ubuntu-specific, but is used on all Debian-based systems like Ubuntu.
  - **snap**: This is developed by Canonical.

## Display server

## Services

## Shell

## Kernel
