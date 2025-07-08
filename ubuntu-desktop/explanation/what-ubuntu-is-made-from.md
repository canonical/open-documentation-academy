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

This ensures that GUI applications can communicate with graphics-related hardware and input devices like the keyboard, mouse, touchscreen, etc.

For example, the Ubuntu Server doesn't ship with a GUI; it runs in headless mode, with no GUI. But it can [install a GUI](https://documentation.ubuntu.com/aws/aws-how-to/instances/launch-ubuntu-desktop/#install-ubuntu-desktop-and-the-snap-store),
and this is possible if your hardware supports graphics components.
When the GUI is installed using `sudo apt-get install -y ubuntu-desktop`, it comes pre-installed with a display server, [desktop environment](#desktop-environment), etc.

The display server consists of a communication protocol and a display server.

- Protocol: Enables communication between the GUI applications and the display server. Examples are [X11](https://en.wikipedia.org/wiki/X_Window_System_core_protocol), [Wayland](https://wayland.freedesktop.org/docs/html/), etc.
- Display server: Implements the protocol. Examples are [X.Org](https://www.x.org/wiki/), [Weston](https://wayland.pages.freedesktop.org/weston/), etc.

Your choice of display server depends on your requirements. For example, you need smoother graphics rendering when gaming or running a modern GPU card.

## Services

These are system applications running in the background. They often automatically start when Ubuntu Desktop boots. You can
manage them using the `systemctl` command-line tool. These applications manage your Wi-Fi, Bluetooth, File System, and other settings.

To list all your services, run:

```shell
systemctl list-units --all --type=service
```

## Shell

A shell is a command-line interface, a non-graphical way to interact with the operating system. You can access it by launching
a Terminal application.

For example, to see your working directory, run `pwd`. To list files in your current directory, you run `ls`.
See [The Linux command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) for more information about Linux commands.

A shell ends with `$` for non-root users and `#` for root users.

There are several types of shells available:

- Bourne Again Shell (bash):
- shell (sh), etc.

You can automate tasks in a shell. To do that, define your task in a shell script that ends with a `.sh` file type. Then, make the script executable and run it.
For example, you can write a script that greets you with hello and displays the time.

## Kernel

This ensures that the Ubuntu operating system can communicate with the entire hardware. It's stored in a disk drive and loaded into RAM when Ubuntu Desktop boots.

Since Ubuntu Desktop is often run as a virtual machine, it means the host system has its own kernel, and so does Ubuntu Desktop.
You can check the path to where the Ubuntu Desktop kernel binary is stored by running:

```bash
ls /boot/vmlinuz-$(uname -r)
```

There are several use cases of a kernel:

- When a display server is launched, its protocol communicates with the kernel, which then directs the request to the GUI-related hardware (GPU, frame buffer, etc.).
- Storage and CPU resources can be allocated to a launched application running [outside the kernel](https://en.wikipedia.org/wiki/User_space_and_kernel_space).
- An application can be loaded to run inside the kernel using [Extended Berkeley Packet Filter (eBPF)](https://documentation.ubuntu.com/server/explanation/intro-to/ebpf/).
