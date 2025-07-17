# What is Ubuntu Desktop made of?

Ubuntu Desktop has millions of users today. As a new user, you might be curious to know what Ubuntu Desktop is made of.
This guide explains the Ubuntu desktop environment, the types of applications it runs, how package managers work, and more.

## Desktop environment

The Ubuntu Desktop environment provides a Graphical User Interface (GUI) for interacting with Ubuntu.
The GUI determines the visual appearance of the desktop and the user experience of interacting with its tools and applications.
Ubuntu Desktop ships with the GNOME desktop environment by default. Other alternative desktop environments include:

- KDE Plasma
- XFCE
- LXQt
- MATE
- Budgie

Interestingly, Ubuntu Desktop comes in [different flavors](https://ubuntu.com/desktop/flavours), each with its desktop environment.
For example, the Kubuntu flavor supports the KDE Plasma desktop environment.

A desktop environment consists of different independent components, including:

- **Windows manager**: This manages a window that pops up when you open a terminal or any application at all.
- **User app**: These are default applications in the desktop environment.
- **File manager**: This is a UI environment for interacting with files.

## Applications

These are comprised of system and user applications.

- **System application**: The system applications interact with the operating system. Examples are the App Center and GNOME Terminal.
- **User applications**: They are used to perform day-to-day activities and have permissions to the operating system restricted. Examples are Firefox and Calculator.

## Package manager

A package manager is used to manage your application lifecycle, including install, upgrade, update and remove. You can do this using a GUI applications or with the command line.

- **The GUI**: Ubuntu Desktop comes pre-installed with an application store called the App Center. It provides a graphical interface that allows you to search for apps and install them.
- **Command line**: Packages can be installed from the terminal using `apt` or `snap` commands:
  - **apt (Advanced Package Tool)**: This is the default package manager on all Debian-based systems like Ubuntu.
  - **snap**: Another built-in package management tool on Ubuntu, which includes all dependencies in a contained environment called a "snap".

## Display server

This ensures that GUI applications can communicate with graphics-related hardware and input devices, including the keyboard, mouse, and touchscreen.


The display server consists of a communication protocol and a display server.

- Protocol: Enables communication between the GUI applications and the display server. Examples are [X11](https://en.wikipedia.org/wiki/X_Window_System_core_protocol) and [Wayland](https://wayland.freedesktop.org/docs/html/).
- Display server: Implements the protocol. Examples are [X.Org](https://www.x.org/wiki/) and [Weston](https://wayland.pages.freedesktop.org/weston/).

Users of Ubuntu Desktop can currently choose between X11 and Wayland.

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

For example, you can run the following:

- `pwd`: Shows your working directory. 
- `ls`: Lists files in your current directory.
See [The Linux command line for beginners](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview) for more information about Linux commands.

The terminal prompt ends with `$` for non-root users and `#` for root users.

There are several types of shells available, including:

- sh
- bash
- fish
- zsh

You can automate tasks in a shell. To do that, define your task in a shell script that ends with a `.sh` file type. Then, make the script executable and run it.
For example, you can write a script that greets you with hello and displays the time.

## Kernel

This ensures that the Ubuntu operating system can communicate with the entire hardware. It's stored in a disk drive and loaded into RAM when Ubuntu Desktop boots.

Since Ubuntu Desktop is often run as a virtual machine, it means the host system has its own kernel, and so does Ubuntu Desktop.
You can check the path to where the Ubuntu Desktop kernel binary is stored by running:

```bash
ls /boot/vmlinuz-$(uname -r)
```

Examples of processes involving the kernel include:

- When a display server is launched, its protocol communicates with the kernel, which then directs the request to the GUI-related hardware (GPU, frame buffer, etc.).
- Storage and CPU resources can be allocated to a launched application running [outside the kernel](https://en.wikipedia.org/wiki/User_space_and_kernel_space).
- An application can be loaded to run inside the kernel using [Extended Berkeley Packet Filter (eBPF)](https://documentation.ubuntu.com/server/explanation/intro-to/ebpf/).
